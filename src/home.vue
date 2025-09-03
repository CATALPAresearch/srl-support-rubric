<template>
  <div data-spy="scroll" data-target="#navbar" data-offset="120px">
    <nav class="container wrapper navbar" style="background-color: white; width:45%; float:right; z-index: 1000;"
      id="navbar">
      <ul class="nav nav-pills pull-right">
        <li role="presentation" class="active"><a href="#">Home</a></li>
        <li role="presentation"><a href="#rubrics">SRL Rubric</a></li>
        <li role="presentation"><a href="#rubricsviz">Visualization</a></li>
        <li role="presentation"><a href="#pub">Publications/citation</a></li>
        <li role="presentation"><a href="#about">About</a></li>
      </ul>
    </nav>

    <div class="container">
      <div class="jumbotron pr-3 pl-3">
        <h1>Self-regulated Learning Support Rubric</h1>
        <p class="lead">
          The integration of advanced learning analytics and data-mining
          technology into higher education has brought various opportunities and
          challenges, particularly in enhancing students' self-regulated learning
          (SRL) skills. Analyzing developed features for SRL support, it has
          become evident that this is not a binary concept but rather a continuum,
          ranging from limited to advanced levels of SRL support. This article
          introduces the rubric, designed to assess the degree of self-regulated
          learning support available within technology enhanced learning
          environments. It is grounded in the theoretical Zimmerman's model and
          employs features distilled from empirical studies that have demonstrated
          significant effectiveness in supporting student self-regulation. Across
          three phases of SRL the rubrics describe assessment criteria and in
          detail define performance levels (Limited, Moderate and Advance). By
          employing the rubric, educators and researchers can 1) gain insights
          into the extent of implemented SRL approaches, 2) further develop SRL
          support of learning environments, and 3) better support students on
          their journey towards becoming self-regulated learners. We report the
          rubric, along with a description of its development process, a report of
          the validation analysis, and conclude with a discussion on the rubric's
          significance in advancing self-regulated learning within the current
          pedagogical and technological landscape.
        </p>
        <p hidden>
          <a class="btn btn-lg btn-success" href="#" role="button">Sign up today</a>
        </p>
      </div>
    </div>


    <div id="rubrics" class="container">
      <h3>SRL Rubrics</h3>
      <div v-for="(phases, p) in rubric" :key="'phase-' + p" class="row"
        :style="'margin-top:50px; background-color:' + phase_colors[p]">
        <h3 style="padding:16px;">{{ phases.phases }}</h3>

        <div v-for="(cat, cat_index) in phases.categories" :key="'cat-' + cat_index" class="col-md-4 carousel slide"
          data-interval="false" data-ride="carousel" :id="'srl-carousel-' + cat_index + '-' + p"
          style="padding-top:6px; border: solid 1px #cccccc50; margin:0px;"
          @slide.bs.carousel="onCarouselSlide($event, p, cat_index)">

          <strong style="margin-left: 50px;">{{ cat.name }}</strong>

          <!-- Display current selection -->
          <div class="current-selection" style="margin-left: 50px; font-size: 12px; color: #666;">
            Selected: Level {{ getSelectedLevel(p, cat_index) }}
          </div>

          <!-- Indicators -->
          <ol class="carousel-indicators">
            <li :data-target="'#srl-carousel-' + cat_index + '-' + p" data-slide-to="0"
              :class="{ active: getSelectedLevel(p, cat_index) === 0 }" @click="setSelectedLevel(p, cat_index, 0)"></li>
            <li :data-target="'#srl-carousel-' + cat_index + '-' + p" data-slide-to="1"
              :class="{ active: getSelectedLevel(p, cat_index) === 1 }" @click="setSelectedLevel(p, cat_index, 1)"></li>
            <li :data-target="'#srl-carousel-' + cat_index + '-' + p" data-slide-to="2"
              :class="{ active: getSelectedLevel(p, cat_index) === 2 }" @click="setSelectedLevel(p, cat_index, 2)"></li>
            <li :data-target="'#srl-carousel-' + cat_index + '-' + p" data-slide-to="3"
              :class="{ active: getSelectedLevel(p, cat_index) === 3 }" @click="setSelectedLevel(p, cat_index, 3)"></li>
          </ol>

          <!-- Wrapper for slides -->
          <div class="carousel-inner">
            <div :class="'item' + (getSelectedLevel(p, cat_index) === 0 ? ' active' : '')"
              style="height: 200px; margin: 0 50px;">
              Level 0. Not supported.
            </div>
            <div v-for="(level, index) in cat.levels" :key="'level-' + index"
              :class="'item' + (getSelectedLevel(p, cat_index) === index + 1 ? ' active' : '')"
              style="height: 200px; margin: 0 50px;">
              Level {{ index + 1 }}. {{ level }}
            </div>
          </div>

          <!-- Left and right controls -->
          <a class="left carousel-control" :href="'#srl-carousel-' + cat_index + '-' + p" data-slide="prev"
            title="Lower SRL support level" @click="decreaseLevel(p, cat_index)">
            <span class="glyphicon glyphicon-chevron-left"></span>
            <span class="sr-only">Previous</span>
          </a>
          <a class="right carousel-control" :href="'#srl-carousel-' + cat_index + '-' + p" data-slide="next"
            title="Higher SRL support level" @click="increaseLevel(p, cat_index)">
            <span class="glyphicon glyphicon-chevron-right"></span>
            <span class="sr-only">Next</span>
          </a>
        </div>
      </div>
    </div>

    <div id="rubricsviz" class="container" style="min-height:740px;">
      <h3>Visualization of the selected SRL Support Rubric levels</h3>
      <RadialBarChart :csv-data="getRadialChartData()" :width="600" :height="600" />
    </div>

    <div id="pub" class="container">
      <h3>Publications</h3>
      <span class="mt-1" style="font-weight: 300;">Our recent publications related to the SRL-S Rubric:</span>
      <ul>
        <li>
          Radović, S., & Seidel, N. (2024). Bridging learning science and learning analytics: Self-Regulation Learning
          support (SRL-S) rubric. Companion Proceedings 14th International Conference on Learning Analytics & Knowledge
          (LAK24), 61–63.
        </li>
        <li>
          Radović, S., & Seidel, N. (2024). Self-regulated learning support in technology enhanced learning
          environments:
          A reliability analysis of the SRL-S Rubric. Journal of Assessment Tools in Education, 11(4), 675–698.
          https://doi.org/10.21449/ijate.1502786
        </li>
      </ul>
      <span class="mt-3" style="font-weight: 300;">How to cite the SRL-S Rubric:</span>
      <pre>Radović, S., & Seidel, N. (2024). A Rubric for Support of Self-Regulated Learning. https://doi.org/10.17605/OSF.IO/WF5S3</pre>
    </div>

    <div id="about" class="container">
      <h3>About</h3>
      This work was developed as part of the CATALPA research group at FernUniversität in Hagen, Germany, in the context
      of the APLE II project. The rubric was created by Dr. Slavisa Radovic and Dr. Niels Seidel. For questions or
      suggestions please contact <a href="mailto:niels.seidel@fernuni-hagen.de">Niels Seidel</a>
    </div>



    <footer class="bg-body-tertiary text-center text-lg-start" style="padding-top:100px;">
      <div style="margin-bottom:50px;">
        <a href="https://www.fernuni-hagen.de/english/research/clusters/catalpa/"><img src="img/promotion/catalpa.jpg"
            width="300" /></a>
        <a href="https://www.fernuni-hagen.de/"><img src="img/promotion/fernuni.jpg" width="250" /></a>
      </div>
      <div class="text-center p-3 mt-3" style="background-color: rgba(0, 0, 0, 0.05);">
        © 2025 CC-BY: Slavisa Radovic & Niels Seidel at <a
          href="https://www.fernuni-hagen.de/english/research/clusters/catalpa/about-catalpa/index.shtml">CATALPA</a>

      </div>

    </footer>
  </div>
</template>

<script>
//import  rubric from './rubric.json' assert { type: 'json' };
import RadialBarChart from './RadialBarChart.vue'

module.exports = {
  components: {
    RadialBarChart
  },
  data: function () {
    return {
      who: "world",
      phase_colors: ['#fde74c20', '#5bc0eb20', '#9bc53d20'],
      // Structure: { phaseIndex: { categoryIndex: selectedLevel } }
      selectedLevels: {},
      rubric: [
        {
          phases: "Forethought",
          categories: [
            {
              name: "F1. Goal Setting",
              levels: [
                "Students acquire course goals predefined by the teacher, they do not have the option to set or modify their learning goals in LE, nor can they easily access goal related performance indicators.",
                "While students still lack the capability to set or change their learning goals themselves in the learning environment itself, however they receive detailed insights about their learning concerning the course's central goal.",
                "Students enjoy the flexibility to choose from a range of learning goals (which may include course mastery or just passing) or to set custom goals (content or performance related). Additionally, LE provides students with details related to the chosen goal.",
              ],
            },
            {
              name: "F2. Strategic Planning",
              levels: [
                "LE primarily focuses on sharing and making learning resources accessible, without including tools to helping students select learning paths, appropriate actions, or plan task execution.",
                "Students are provided with an overview of all available learning resources (those completed, left unfinished, or which are next), allowing them to quickly access, prioritize tasks and identify the materials they need.",
                "LE presents students with an overview of all learning resources, along with valuable information such as success rates, progress tracking, or estimated time required for each resource. This empowers students to plan their next learning activities effectively, priorities tasks, and estimate effort required.",
              ],
            },
            {
              name: "F3. Self-Efficacy",
              levels: [
                "Students are only provided with minimal information (usually midterm) regarding their past performance (or success, progress, effort, time spent, etc.). LE do not actively fostering beliefs in one's capabilities.",
                "LE offers detailed information about the students' own efficacy (performance, progress, effort, etc.) or prompts them to reflect on their self-perceived efficacy. This enables students to assess their capabilities.",
                "LE provides students with comprehensive details about their efficacy or prompts them to reflect on their self-perceived efficacy. LE goes a step further by offering predictions (about success, outcomes, time needed, etc.) to empower students to set realistic expectations and strategic decisions.",
              ],
            },
            {
              name: "F4. Task Value",
              levels: [
                "The focus in LE is on completing assignments with limited practical application or connection to next learning chapters (nor other subject or courses).",
                "LE follows the principles of authenticity, allowing students to apply their knowledge to solve realistic practice assignments. This raises the value of learning content.",
                "Students not only engage with authentic LE, but advanced learning technologies allows students to use professional tools, skills, or relevant methods (for their study or selected goal) to develop and self-assess knowledge.",
              ],
            },
            {
              name: "F5. Goal Orientation",
              levels: [
                "LE does only provide general information regarding the course requirements (goal set by teacher). Students lack visibility into how they are performing or advancing towards their desired goals.",
                "LE offers students’ criteria for success and displays their performance in relation to the goal (set by teacher). Students can compare their progress and performance against the predefined criteria and the goal.",
                "LE goes beyond providing information about students' progress, process, and outcome in relation to their goals. It also visualizes what and how needs to be improved or adjusted to attain the selected goal.",
              ],
            },
          ],
        },
        {
          phases: "Performance",
          categories: [
            {
              name: "P1. Self-Instruction",
              levels: [
                "The LE provides outline and table of content (including all different material). Besides, there are general instructions about the course requirements to helps individuals take control of their learning.",
                "LE provides task-specific or general self-questions along the learning resource to encourage students to achieve desired outcomes. This allows students to have better understanding of the expectations and increases learning motivation.",
                "The LE provides adaptive self-instruction cues that directed cognitive process that helps students take control of their learning. A technology (like intelligent chatbot or similar) uses motivational technique and describes steps in the coping process.",
              ],
            },
            {
              name: "P2. Imagery",
              levels: [
                "LE uses images and visual representation of learning material to support the forming of vivid mental pictures and visual models.",
                "LE additionally includes videos and tools for graphical strategies within the text (annotations, color-coded text, and similar visual aids are utilized to enhance knowledge organization).",
                "LE provides interactive simulations or virtual reality space for developing knowledge and practicing skills. LE could also support students in creating concept maps and create their own visualizations.",
              ],
            },
            {
              name: "P3. Time Management",
              levels: [
                "There is a very limited support provided for time management within LE e.g., only mentioning deadlines and exam dates. Le do not record time spent on learning activities.",
                "Students have access to information about their past performance as well as the time spent on specific learning resources or overall learning. Automatics deadlines reminders are sent to students.",
                "Students receive information about their past behavior (or success, progress, time, etc.), but also receive future predictions on managing time effectively in relation to their selected goals.",
              ],
            },
            {
              name: "P4. Help Seeking",
              levels: [
                "Beside direct communication with the teacher, students may not have clear avenues or guidance to seek assistance when faced with challenges.",
                "Students can engage with peers and teachers, to ask questions, share concerns, or request support (e.g., peer review). LE offers a/synch communication channels (forum, chat, LMS tools, etc.) which students can use.",
                "Students are specifically instructed and supported to use various communication channels (e.g., tasks shared with peers, collaborative joint activities). Additionally, established help seeking support system includes external resources, AI agents, or querying LLM.",
              ],
            },
            {
              name: "P5. Task Strategies",
              levels: [
                "LE does provide a general description of different strategies that can be used. There is no specific structure to support students in performing different tasks.",
                "There are task-related strategies provided during the student activity of solving or self-assessing task solutions. Students are supported in redoing tasks using alternative strategies.",
                "LE offers task-specific strategies for assessing different tasks (this can include tips on critical thinking, summarization, application of skills). Moreover, LE provide feedback on students’ learning strategies, behavior, and effective strategies etc.",
              ],
            },
            {
              name: "P6. Metacognitive Monitoring",
              levels: [
                "Students can test their knowledge by solving tasks and manually scoring their results. There is a lack of support for analytics, monitoring understanding, and evaluating success of chosen strategies.",
                "LE supports students in monitoring their progress in relation to general course outcomes. Students can gauge their overall performance (or success, progress, etc.) against the formal objectives of the course.",
                "LE enables students to compare their progress globally, but also in relation to learning units, specific materials (e.g., texts, tasks, reflections), and individual items. Additionally, LE provides monitoring of one’s own SRL behavior, used strategies, and learning patterns.",
              ],
            },
          ],
        },
        {
          phases: "Self-reflection",
          categories: [
            {
              name: "S1. Self-Evaluation",
              levels: [
                "LE provides a sample solution that may help students to self-evaluate their solution against one possible solution (feed-up). However, it does not support identification of areas for improvement.",
                "LE provides different types of tasks that allow students to evaluate their knowledge and skills through, for example, various assessments, self-assessments, or quizzes (feed-back).",
                "LE provides tasks for self-evaluation, but also offers additional data analysis, feedback and guidance on specific areas that need attention (feed-forward). E.g. it visualizes and marks tasks that are underperformed in relation to the learning goal, identify requiring improvement.",
              ],
            },
            {
              name: "S2. Causal Attribution",
              levels: [
                "Students are offered a limited resources to reflect (e.g., knowledge tests and corresponding rubrics). They are not guided nor supported how to reflect on performance or how to evaluate factors of failure.",
                "Students are asked to think about their performance when self-assessing their tasks’ solutions against criteria. This level of support encourages students to consider the factors that influenced their performance.",
                "LE includes prompted critical reflection tasks after major learning events or learning units. These tasks ask students to think in several directions – about their performance, their strengths and weaknesses, or to assess their progress toward their goals.",
              ],
            },
            {
              name: "S3. Self-Reactions",
              levels: [
                "Students are offered with resources to engage in reflecting on their learning experiences, emotions, or future goals. This often only includes tasks for knowledge tests and corresponding rubrics.",
                "LE incorporates strategies that allows students to gain insights into their learning process. This level of support enables students’ awareness and reflection on their learning activities.",
                "LE goes beyond providing a learning analytics dashboard. It includes critical reflection tasks that specifically ask students to reflect on their learning experiences, identify future goals, or think about their feelings of satisfaction or disappointment.",
              ],
            },
            {
              name: "S4. Adaptation",
              levels: [
                "LE does offer limited guidance or resources to assist students in modifying or adapting their approaches to learning. This is usually organized as scheduled virtual cohort meetings (initial, midterm, before exam) with teachers.",
                "LE provides simple support for students’ adaptation. Students can gain insights into their learning progress; however, they are not supported and do not have the option to change their goals directly within the LE.",
                "LE includes critical reflection assignments that specifically ask students to reflect on adjusting their learning strategies, setting new goal within LE, and planning for future learning. Students are prompted to monitor their progress and areas for improvement, to adapt their strategies.",
              ],
            },
          ],
        },
      ],
    };
  },
  mounted() {
    // Initialize selected levels when component mounts
    this.initializeSelectedLevels()
  },

  watch: {
    // Watch for changes in rubric data and reinitialize
    rubric: {
      handler() {
        this.initializeSelectedLevels()
      },
      deep: true
    }
  },

  methods: {
    // Initialize selected levels with default values (level 1)
    initializeSelectedLevels() {
      const levels = {}

      this.rubric.forEach((phase, phaseIndex) => {
        levels[phaseIndex] = {}
        phase.categories.forEach((category, categoryIndex) => {
          // Default to level 0 (you can change this default)
          levels[phaseIndex][categoryIndex] = 0
        })
      })

      this.selectedLevels = levels
    },

    // Get selected level for a specific phase and category
    getSelectedLevel(phaseIndex, categoryIndex) {
      if (!this.selectedLevels[phaseIndex]) {
        return 0 // default
      }
      return this.selectedLevels[phaseIndex][categoryIndex] || 0
    },

    // Set selected level for a specific phase and category
    setSelectedLevel(phaseIndex, categoryIndex, level) {
      if (!this.selectedLevels[phaseIndex]) {
        this.selectedLevels[phaseIndex] = {}
      }

      this.selectedLevels[phaseIndex][categoryIndex] = level

      // Force component to update
      this.$forceUpdate()

      this.$emit('level-changed', { phaseIndex, categoryIndex, level, allSelections: this.selectedLevels })
    },

    // Handle carousel slide events
    onCarouselSlide(event, phaseIndex, categoryIndex) {
      // Get the slide index from the event
      const slideIndex = event.to || 0
      this.setSelectedLevel(phaseIndex, categoryIndex, slideIndex)
    },

    // Increase level (right arrow)
    increaseLevel(phaseIndex, categoryIndex) {
      //console.log(phaseIndex, categoryIndex)
      const currentLevel = this.getSelectedLevel(phaseIndex, categoryIndex)
      const maxLevel = this.getMaxLevel(phaseIndex, categoryIndex)
      //console.log(currentLevel, maxLevel)

      if (currentLevel < maxLevel) {
        this.setSelectedLevel(phaseIndex, categoryIndex, currentLevel + 1)
      }
    },

    // Decrease level (left arrow)
    decreaseLevel(phaseIndex, categoryIndex) {
      const currentLevel = this.getSelectedLevel(phaseIndex, categoryIndex)

      if (currentLevel > 0) {
        this.setSelectedLevel(phaseIndex, categoryIndex, currentLevel - 1)
      }
    },

    // Get maximum level for a category (based on available levels)
    getMaxLevel(phaseIndex, categoryIndex) {
      if (!this.rubric[phaseIndex] || !this.rubric[phaseIndex].categories[categoryIndex]) {
        return 3
      }

      const category = this.rubric[phaseIndex].categories[categoryIndex]
      return category.levels ? category.levels.length : 3
    },

    // Get all selections in a structured format
    getAllSelections() {
      return this.selectedLevels
    },

    // Reset all selections to default
    resetSelections() {
      this.initializeSelectedLevels()
    },

    // Load selections from external data
    loadSelections(selections) {
      this.selectedLevels = selections
    },

    getRadialChartData() {
      const csvData = []

      // Iterate through each phase in the rubric
      this.rubric.forEach((phase, phaseIndex) => {
        const phaseName = phase.phases // e.g., "Forethought Phase"

        // Iterate through each category in the current phase
        phase.categories.forEach((category, categoryIndex) => {
          const categoryName = category.name // e.g., "Goal Setting"
          const selectedLevel = this.getSelectedLevel(phaseIndex, categoryIndex)

          // Create data object in CSV format
          csvData.push({
            category_label: phaseName,
            question_label: categoryName,
            value: selectedLevel
          })
        })
      })

      return csvData
    },

    convertToCSV() {
      const data = this.getRadialChartData()

      // Create header row
      const headers = 'category_label,question_label,value'

      // Convert each data row to CSV format
      const rows = data.map(row => {
        return `${row.category_label},${row.question_label},${row.value}`
      })

      // Combine header and rows
      return headers + '\n' + rows.join('\n')
    }

  }
};
</script>

<style>
/* Space out content a bit */
body {
  padding-top: 20px;
  padding-bottom: 20px;
  font-family: "Roboto", sans-serif;
  position: relative;
}

/* Everything but the jumbotron gets side spacing for mobile first views */
.header,
.marketing,
.footer {
  padding-right: 15px;
  padding-left: 15px;
}

/* Custom page header */
.header {
  padding-bottom: 20px;
  border-bottom: 1px solid #e5e5e5;
}

.wrapper {
  position: sticky;
  top: 0;
}

/* Make the masthead heading the same height as the navigation */
.header h3 {
  margin-top: 0;
  margin-bottom: 0;
  line-height: 40px;
}

/* Custom page footer */
.footer {
  padding-top: 19px;
  color: #777;
  border-top: 1px solid #e5e5e5;
}

/* Customize container */
@media (min-width: 768px) {
  .container {}
}

.container-narrow>hr {
  margin: 30px 0;
}

/* Main marketing message and sign up button */
.jumbotron {
  text-align: left;
  border-bottom: 1px solid #e5e5e5;
  color: #333;
}

.jumbotron h1 {
  font-size: 32px;
}

.jumbotron p {
  font-size: 18px;
  font-weight: 300;
}

.jumbotron .btn {
  padding: 14px 24px;
  font-size: 16px;
}

/* Supporting marketing content */
.marketing {
  margin: 40px 0;
}

.marketing p+h4 {
  margin-top: 28px;
}

.carousel-control.right,
.carousel-control.left {
  background-image: linear-gradient(to right, rgba(0, 0, 0, .0001) 0, rgba(0, 0, 0, .1) 100%);
  background: none;
}

/* Responsive: Portrait tablets and up */
@media screen and (min-width: 768px) {

  /* Remove the padding we set earlier */
  .header,
  .marketing,
  .footer {
    padding-right: 0;
    padding-left: 0;
  }

  /* Space out the masthead */
  .header {
    margin-bottom: 30px;
  }

  /* Remove the bottom border on the jumbotron for visual effect */
  .jumbotron {
    border-bottom: 0;
  }
}
</style>
