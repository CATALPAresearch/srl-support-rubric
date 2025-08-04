<template>
  <div class="container">
    <div class="header clearfix">
      <nav>
        <ul hidden class="nav nav-pills pull-right">
          <li role="presentation" class="active"><a href="#">Home</a></li>
          <li role="presentation"><a href="#">About</a></li>
          <li role="presentation"><a href="#">Contact</a></li>
        </ul>
      </nav>
      <h3 hidden class="text-muted">APLE II, CATALPA</h3>
    </div>

    <div class="jumbotron">
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
        <a class="btn btn-lg btn-success" href="#" role="button"
          >Sign up today</a
        >
      </p>
    </div>

    <div class="container">
      <div v-for="(phases,p) in rubric" keay="phase" class="row" :style="'margin-top:50px; background-color:'+phase_colors[p] ">
        <h3 style="padding:16px;">{{ phases.phases }}</h3>
        <div
          v-for="(cat, cat_index) in phases.categories"
          key="phase"
          class="col-md-4 carousel slide"
          data-interval="false"
          data-ride="carousel"
          :id="'srl-carousel-' + cat_index + '-' + p"
          style="padding-top:6px; border: solid 1px #cccccc50; margin:0px;"
        >
          <strong style="margin-left: 50px;">{{ cat.name }}</strong>
          <!-- Indicators -->
          <ol class="carousel-indicators">
            <li
              :data-target="'#srl-carousel-' + cat_index + '-' + p"
              data-slide-to="0"
            ></li>
            <li
              :data-target="'#srl-carousel-' + cat_index + '-' + p"
              data-slide-to="1"
              class="active"
            ></li>
            <li
              :data-target="'#srl-carousel-' + cat_index + '-' + p"
              data-slide-to="2"
            ></li>
            <li
              :data-target="'#srl-carousel-' + cat_index + '-' + p"
              data-slide-to="3"
            ></li>
          </ol>

          <!-- Wrapper for slides -->
          <div class="carousel-inner">
            <div
              class="item"
              style="height: 200px; margin: 0 50px;"
            >
              Level 0. Not supported.
            </div>
            <div
              v-for="(level, index) in cat.levels"
              key="level"
              :class="'item' + (index == 0 ? ' active' : '')"
              style="height: 200px; margin: 0 50px;"
            >
              Level {{ index + 1 }}. {{ level }}
            </div>
          </div>

          <!-- Left and right controls -->
          <a
            class="left carousel-control"
            :href="'#srl-carousel-' + cat_index + '-' + p"
            data-slide="prev"
            title="Lower SRL support level"
          >
            <span class="glyphicon glyphicon-chevron-left"></span>
            <span class="sr-only">Previous</span>
          </a>
          <a
            class="right carousel-control"
            :href="'#srl-carousel-' + cat_index + '-' + p"
            data-slide="next"
            title="Higher SRL support level"
          >
            <span class="glyphicon glyphicon-chevron-right"></span>
            <span class="sr-only">Next</span>
          </a>
        </div>
      </div>
    </div>
  </div>
  <footer class="bg-body-tertiary text-center text-lg-start" style="padding-top:100px;">
    
    <div class="text-center p-3" style="background-color: rgba(0, 0, 0, 0.05);">
      © 2025 Copyright: Slavisa Radovic & Niels Seidel at <a href="https://www.fernuni-hagen.de/english/research/clusters/catalpa/about-catalpa/index.shtml">CATALPA</a>

    </div>
    
  </footer>

</template>

<script>
//import  rubric from './rubric.json' assert { type: 'json' };
module.exports = {
  data: function () {
    return {
      who: "world",
      phase_colors: ['#fde74c20', '#5bc0eb20', '#9bc53d20'],
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
  mounted: function () {},
};
</script>

<style>
/* Space out content a bit */
body {
  padding-top: 20px;
  padding-bottom: 20px;
  font-family: "Roboto", sans-serif;
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
  .container {
  }
}
.container-narrow > hr {
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
.marketing p + h4 {
  margin-top: 28px;
}

.carousel-control.right, .carousel-control.left{
  background-image: linear-gradient(to right,rgba(0,0,0,.0001) 0,rgba(0,0,0,.1) 100%);
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
