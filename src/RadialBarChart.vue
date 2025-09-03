<template>
  <div class="radial-chart-container">
    <div ref="chartContainer" class="chart-container" style="display: block;">
      <!-- Chart will be rendered here -->
    </div>
    
    <div v-if="error" class="error-message" v-html="error"></div>
  </div>
</template>

<script>
export default {
  name: 'RadialBarChart',
  props: {
    csvData: {
      type: Array,
      default: null      
    },
    width: {
      type: Number,
      default: 600
    },
    height: {
      type: Number,
      default: 600
    }
  },
  data() {
    return {
      error: null,
      d3Loaded: false
    }
  },
  async mounted() {
    await this.loadD3()
    if (this.d3Loaded) {
      this.initChart()
    }
  },
  watch: {
    // Watch for changes in csvData prop and re-render chart
    csvData: {
      handler() {
        if (this.d3Loaded && this.csvData) {
          this.initChart()
        }
      },
      deep: true
    }
  },
  beforeUnmount() {
    // Clean up any existing chart
    if (window.d3 && this.$refs.chartContainer) {
      window.d3.select(this.$refs.chartContainer).selectAll("*").remove()
    }
  },
  methods: {
    async loadD3() {
      return new Promise((resolve, reject) => {
        if (window.d3) {
          this.d3Loaded = true
          resolve()
          return
        }
        
        const script = document.createElement('script')
        script.src = 'https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.17/d3.min.js'
        script.onload = () => {
          this.d3Loaded = true
          resolve()
        }
        script.onerror = () => {
          this.error = 'Failed to load D3.js library'
          reject(new Error('Failed to load D3.js'))
        }
        document.head.appendChild(script)
      })
    },
    
    initChart() {
      if (!this.csvData || this.csvData.length === 0) {
        this.error = "No data provided to chart"
        return
      }
      this.draw(this.csvData)
    },
    
    draw(data) {
      "use strict";
      
      const d3 = window.d3
      if (!d3) {
        this.error = 'D3.js library not loaded'
        return
      }

      // Clear any existing chart and error
      d3.select(this.$refs.chartContainer).selectAll("*").remove()
      this.error = null

      // Validate data structure
      if (!this.validateData(data)) {
        this.error = "Invalid data format. Expected array of objects with category_label, question_label, and value properties."
        return
      }

      var margin = 0,
        width = this.width,
        height = this.height,
        maxBarHeight = height / 2 - (margin + 70);

      var innerRadius = 0.1 * maxBarHeight;

      var svg = d3.select(this.$refs.chartContainer)
        .append("svg")
        .attr("width", width)
        .attr("height", height)
        .append("g")
        .attr("class", "chart")
        .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

      this.createGradients(svg, maxBarHeight);
      this.createCircles(svg, maxBarHeight, innerRadius);

      // Process the data
      var cats = data.map(function(d) {
        return d.category_label;
      });

      var catCounts = {};
      for (var i = 0; i < cats.length; i++) {
        var num = cats[i];
        catCounts[num] = catCounts[num] ? catCounts[num] + 1 : 1;
      }
      
      // Remove duplicates
      cats = cats.filter(function(v, i) {
        return cats.indexOf(v) == i;
      });
      var numCatBars = cats.length;

      var angle = 0,
        rotate = 0;

      // Calculate angles for each data point
      data.forEach(function(d) {
        d.startAngle = angle;
        angle += (2 * Math.PI) / numCatBars / catCounts[d.category_label];
        d.endAngle = angle;
        d.rotate = rotate;
        rotate += 360 / numCatBars / catCounts[d.category_label];
      });

      // Create category labels
      this.createCategoryLabels(svg, cats, numCatBars, maxBarHeight);
      
      // Create question labels
      this.createQuestionLabels(svg, data, maxBarHeight);

      // Create bars
      this.createBars(svg, data, innerRadius, maxBarHeight);

      // Create gridlines
      this.createGridlines(svg, data, cats, innerRadius, maxBarHeight, numCatBars);
    },

    validateData(data) {
      if (!Array.isArray(data) || data.length === 0) {
        return false;
      }
      
      return data.every(item => 
        item.hasOwnProperty('category_label') && 
        item.hasOwnProperty('question_label') && 
        item.hasOwnProperty('value') &&
        typeof item.value === 'number'
      );
    },

    createGradients(svg, maxBarHeight) {
      var defs = svg.append("defs");

      // Chart area gradient
      var gradients = defs
        .append("linearGradient")
        .attr("id", "gradient-chart-area")
        .attr("x1", "50%")
        .attr("y1", "0%")
        .attr("x2", "50%")
        .attr("y2", "100%")
        .attr("spreadMethod", "pad");

      gradients.append("stop")
        .attr("offset", "0%")
        .attr("stop-color", "#EDF0F0")
        .attr("stop-opacity", 1);

      gradients.append("stop")
        .attr("offset", "100%")
        .attr("stop-color", "#ACB7BE")
        .attr("stop-opacity", 1);

      // Questions gradient
      gradients = defs
        .append("linearGradient")
        .attr("id", "gradient-questions")
        .attr("x1", "50%")
        .attr("y1", "0%")
        .attr("x2", "50%")
        .attr("y2", "100%")
        .attr("spreadMethod", "pad");

      gradients.append("stop")
        .attr("offset", "0%")
        .attr("stop-color", "#F6F8F9")
        .attr("stop-opacity", 1);

      gradients.append("stop")
        .attr("offset", "100%")
        .attr("stop-color", "#D4DAE0")
        .attr("stop-opacity", 1);

      // Bars gradient
      gradients = defs
        .append("radialGradient")
        .attr("id", "gradient-bars")
        .attr("gradientUnits", "userSpaceOnUse")
        .attr("cx", "0")
        .attr("cy", "0")
        .attr("r", maxBarHeight)
        .attr("spreadMethod", "pad");

      gradients.append("stop")
        .attr("offset", "0%")
        .attr("stop-color", "#F3D5AA");

      gradients.append("stop")
        .attr("offset", "50%")
        .attr("stop-color", "#F4A636");

      gradients.append("stop")
        .attr("offset", "100%")
        .attr("stop-color", "#AF4427");
    },

    createCircles(svg, maxBarHeight, innerRadius) {
      svg.append("circle")
        .attr("r", maxBarHeight + 70)
        .classed("category-circle", true);

      svg.append("circle")
        .attr("r", maxBarHeight + 40)
        .classed("question-circle", true);

      svg.append("circle")
        .attr("r", maxBarHeight)
        .classed("chart-area-circle", true);

      svg.append("circle")
        .attr("r", innerRadius)
        .classed("center-circle", true);
    },

    createCategoryLabels(svg, cats, numCatBars, maxBarHeight) {
      const d3 = window.d3;
      
      var arc_category_label = d3.svg.arc()
        .startAngle(function(d, i) {
          return (i * 2 * Math.PI) / numCatBars;
        })
        .endAngle(function(d, i) {
          return ((i + 1) * 2 * Math.PI) / numCatBars;
        })
        .innerRadius(maxBarHeight + 40)
        .outerRadius(maxBarHeight + 64);

      var category_text = svg.selectAll("path.category_label_arc")
        .data(cats)
        .enter().append("path")
        .classed("category-label-arc", true)
        .attr("id", function(d, i) {
          return "category_label_" + i;
        })
        .attr("fill", "none")
        .attr("d", arc_category_label);

      // Process arc paths for text
      category_text.each(function(d, i) {
        var firstArcSection = /(^.+?)L/;
        var arcPath = d3.select(this).attr("d");
        if (!arcPath) return;
        
        var match = firstArcSection.exec(arcPath);
        if (!match) return;
        
        var newArc = match[1].replace(/,/g, " ");
        var startAngle = (i * 2 * Math.PI) / numCatBars;
        var endAngle = ((i + 1) * 2 * Math.PI) / numCatBars;

        if (startAngle > Math.PI / 2 && startAngle < 3 * Math.PI / 2 && 
            endAngle > Math.PI / 2 && endAngle < 3 * Math.PI / 2) {
          var startLoc = /M(.*?)A/;
          var middleLoc = /A(.*?)0 0 1/;
          var endLoc = /0 0 1 (.*?)$/;
          
          var startMatch = startLoc.exec(newArc);
          var middleMatch = middleLoc.exec(newArc);
          var endMatch = endLoc.exec(newArc);
          
          if (startMatch && middleMatch && endMatch) {
            var newStart = endMatch[1];
            var newEnd = startMatch[1];
            var middleSec = middleMatch[1];
            newArc = "M" + newStart + "A" + middleSec + "0 0 0 " + newEnd;
          }
        }
        d3.select(this).attr("d", newArc);
      });

      // Add category text labels
      svg.selectAll(".category-label-text")
        .data(cats)
        .enter().append("text")
        .attr("class", "category-label-text")
        .attr("dy", function(d, i) {
          var startAngle = (i * 2 * Math.PI) / numCatBars;
          var endAngle = ((i + 1) * 2 * Math.PI) / numCatBars;
          return (startAngle > Math.PI / 2 && startAngle < 3 * Math.PI / 2 && 
                 endAngle > Math.PI / 2 && endAngle < 3 * Math.PI / 2 ? -4 : 14);
        })
        .append("textPath")
        .attr("startOffset", "50%")
        .style("text-anchor", "middle")
        .attr("xlink:href", function(d, i) {
          return "#category_label_" + i;
        })
        .text(function(d) {
          return d;
        });
    },

    createQuestionLabels(svg, data, maxBarHeight) {
      const d3 = window.d3;
      
      var arc_question_label = d3.svg.arc()
        .startAngle(function(d) {
          return d.startAngle;
        })
        .endAngle(function(d) {
          return d.endAngle;
        })
        .outerRadius(maxBarHeight + 2);

      var question_text = svg.selectAll("path.question_label_arc")
        .data(data)
        .enter().append("path")
        .classed("question-label-arc", true)
        .attr("id", function(d, i) {
          return "question_label_" + i;
        })
        .attr("fill", "none")
        .attr("d", arc_question_label);

      // Process question arcs
      question_text.each(function(d, i) {
        var firstArcSection = /(^.+?)L/;
        var arcPath = d3.select(this).attr("d");
        if (!arcPath) return;
        
        var match = firstArcSection.exec(arcPath);
        if (!match) return;
        
        var newArc = match[1].replace(/,/g, " ");

        if (d.startAngle > Math.PI / 2 && d.startAngle < 3 * Math.PI / 2 && 
            d.endAngle > Math.PI / 2 && d.endAngle < 3 * Math.PI / 2) {
          var startLoc = /M(.*?)A/;
          var middleLoc = /A(.*?)0 0 1/;
          var endLoc = /0 0 1 (.*?)$/;
          
          var startMatch = startLoc.exec(newArc);
          var middleMatch = middleLoc.exec(newArc);
          var endMatch = endLoc.exec(newArc);
          
          if (startMatch && middleMatch && endMatch) {
            var newStart = endMatch[1];
            var newEnd = startMatch[1];
            var middleSec = middleMatch[1];
            newArc = "M" + newStart + "A" + middleSec + "0 0 0 " + newEnd;
          }
        }
        d3.select(this).attr("d", newArc);
      });

      // Add question text labels
      var question_text_labels = svg.selectAll(".question-label-text")
        .data(data)
        .enter().append("text")
        .attr("class", "question-label-text")
        .append("textPath")
        .style('font-size', '7px')
        .style('font-family', 'sans-serif')
        .attr("xlink:href", function(d, i) {
          return "#question_label_" + i;
        })
        .text(function(d) {
          return d.question_label.toUpperCase();
        })
        .call(this.wrapTextOnArc.bind(this), maxBarHeight);

      // Adjust text positioning
      question_text_labels.each(function(d, i) {
        var textPath = d3.select(this)[0][0];
        if (!textPath || !textPath.childNodes) return;
        
        var tspanCount = textPath.childNodes.length;
        if (tspanCount > 0 && textPath.childNodes[0]) {
          var dyValue = d.startAngle > Math.PI / 2 && d.startAngle < 3 * Math.PI / 2 && 
                       d.endAngle > Math.PI / 2 && d.endAngle < 3 * Math.PI / 2 ? 
                       3 + (tspanCount - 1) * -0.6 + 'em' : 
                       -2.1 + (tspanCount - 1) * -0.6 + 'em';
          d3.select(textPath.childNodes[0]).attr("dy", dyValue);
        }
      });
    },

    createBars(svg, data, innerRadius, maxBarHeight) {
      const d3 = window.d3;
      
      var x_scale = d3.scale.linear()
        .domain([0, 3])
        .range([innerRadius, maxBarHeight]);

      var arc = d3.svg.arc()
        .startAngle(function(d) {
          return d.startAngle;
        })
        .endAngle(function(d) {
          return d.endAngle;
        })
        .innerRadius(innerRadius);

      var bars = svg.selectAll("path.bar")
        .data(data)
        .enter().append("path")
        .classed("bars", true)
        .each(function(d) {
          d.outerRadius = innerRadius;
        })
        .attr("d", arc);

      // Animate bars
      bars.transition().ease("elastic").duration(1000).delay(function(d, i) {
          return i * 100;
        })
        .attrTween("d", function(d, index) {
          var i = d3.interpolate(d.outerRadius, x_scale(+d.value));
          return function(t) {
            d.outerRadius = i(t);
            return arc(d, index);
          };
        });
    },

    createGridlines(svg, data, cats, innerRadius, maxBarHeight, numCatBars) {
      const d3 = window.d3;
      
      var x_scale = d3.scale.linear()
        .domain([0, 3])
        .range([innerRadius, maxBarHeight]);

      var y_scale = d3.scale.linear()
        .domain([0, 3])
        .range([-innerRadius, -maxBarHeight]);

      // Radial gridlines
      svg.selectAll("circle.x.minor")
        .data(y_scale.ticks(3))
        .enter().append("circle")
        .classed("gridlines minor", true)
        .attr("r", function(d) {
          return x_scale(d);
        });

      // Question lines
      svg.selectAll("line.y.minor")
        .data(data)
        .enter().append("line")
        .classed("gridlines minor", true)
        .attr("y1", -innerRadius)
        .attr("y2", -maxBarHeight - 40)
        .attr("transform", function(d) {
          return "rotate(" + d.rotate + ")";
        });

      // Category lines
      svg.selectAll("line.y.major")
        .data(cats)
        .enter().append("line")
        .classed("gridlines major", true)
        .attr("y1", -innerRadius)
        .attr("y2", -maxBarHeight - 70)
        .attr("transform", function(d, i) {
          return "rotate(" + (i * 360 / numCatBars) + ")";
        });
    },

    wrapTextOnArc(text, radius) {
      const d3 = window.d3
      if (!d3) return
      
      var temporaryText = d3.select(this.$refs.chartContainer).select('svg')
        .append("text")
        .attr("class", "temporary-text")
        .style("font", "7px sans-serif")
        .style("opacity", 0);

      var getTextLength = function(string) {
        temporaryText.text(string);
        return temporaryText.node().getComputedTextLength();
      };

      text.each(function(d) {
        var text = d3.select(this),
          words = text.text().split(/[ \f\n\r\t\v]+/).reverse(),
          word,
          line = [],
          textLength,
          lineHeight = 1.1,
          x = 0,
          y = 0,
          dy = 0,
          tspan = text.text(null).append("tspan").attr("x", x).attr("y", y).attr("dy", dy + "em"),
          arcLength = ((d.endAngle - d.startAngle) / (2 * Math.PI)) * (2 * Math.PI * radius),
          paddedArcLength = arcLength - 16;

        while (word = words.pop()) {
          line.push(word);
          tspan.text(line.join(" "));
          textLength = getTextLength(tspan.text());
          tspan.attr("x", (arcLength - textLength) / 2);

          if (textLength > paddedArcLength && line.length > 1) {
            line.pop();
            tspan.text(line.join(" "));
            textLength = getTextLength(tspan.text());
            tspan.attr("x", (arcLength - textLength) / 2);

            line = [word];
            tspan = text.append("tspan").attr("dy", lineHeight + dy + "em").text(word);
            textLength = getTextLength(tspan.text());
            tspan.attr("x", (arcLength - textLength) / 2);
          }
        }
      });

      d3.selectAll("text.temporary-text").remove();
    }
  }
}
</script>

<style scoped>
.radial-chart-container {
  width: 100%;
  height: 100%;
  font-family: Arial, sans-serif;
}

.chart-container {
  background: white;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  padding: 20px;
  margin: 20px 0;
  text-align: center;
}

.error-message {
  color: red;
  padding: 20px;
  background: #ffe6e6;
  border-radius: 5px;
  margin: 20px 0;
}

/* Chart-specific styles using :deep() */
:deep(.category-circle) {
  fill: url(#gradient-chart-area);
  stroke: none;
}

:deep(.question-circle) {
  fill: url(#gradient-questions);
  stroke: none;
}

:deep(.chart-area-circle) {
  fill: transparent;
  stroke: #ccc;
  stroke-width: 1px;
}

:deep(.center-circle) {
  fill: white;
  stroke: #ccc;
  stroke-width: 1px;
}

:deep(.bars) {
  fill: url(#gradient-bars);
  stroke: white;
  stroke-width: 1px;
}

:deep(.gridlines.minor) {
  stroke: rgba(255, 255, 255, 0.5);
  stroke-width: 0.5px;
  fill: none;
}

:deep(.gridlines.major) {
  stroke: rgba(255, 255, 255, 0.8);
  stroke-width: 1px;
  fill: none;
}

:deep(.category-label-text) {
  font: 12px sans-serif;
  fill: #333;
  font-weight: bold;
}

:deep(.question-label-text) {
  font: 7px sans-serif;
  fill: #666;
}
</style>