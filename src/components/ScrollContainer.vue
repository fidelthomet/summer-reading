<template>
  <div class="scroll-container">
    <figure ref="figure" v-resize:debounce.initial="resize">
      <TextGraphic :width="width" :height="height" :step="step" :progress="progress" :item="item"/>
    </figure>
    <article>
      <div class="step">
        <div class="inner">
          <h1>InfoVis Papers to Read on Your Summer Staycation</h1>
          <p>
            <strong>about →</strong>
            Information visualization (InfoVis) is a dynamic field of research and practice that is concerned with the design, development, and evaluation of interactive and visual representations of data. InfoVis research provides a great basis for interface designers, researchers, and developers to create effective data visualizations and study their potential. Recent InfoVis research connects well with contemporary application areas of interface design such as data-based storytelling, generative art, critical design, science communication, digital humanities, algorithmic accountability, social advocacy, and climate activism. Conceptualized as a self-organized reading group, this weekly seminar provides a supportive platform to read and discuss current InfoVis papers together. Students are encouraged to select the readings according to their own interests and apply insights and ideas from the readings and discussions to their own projects. The goal of the seminar is to critically engage with academic literature, transfer insights to practical design work, and devise new ideas for future research projects.
          </p>
        </div>
      </div>
      <div class="step" v-for="(p, i) in top" :key="`p-${i}`">
        <div class="inner">
          <h3>{{p.authors}}</h3>
          <h2>{{p.title}}</h2>
          <h4>{{p.venue}}, {{p.pages}}, {{p.year}}</h4>
          <p>{{ p.abstract }}</p>

          <a class="button" :href="p.url">→ Access Paper</a>
        </div>
      </div>
    </article>
  </div>
</template>

<script>
import scrollama from "scrollama";
import resize from "vue-resize-directive";
import TextGraphic from "@/components/TextGraphic";
import top from "@/assets/data/top.json";
export default {
  name: "scroll-container",
  directives: {
    resize
  },
  components: {
    TextGraphic
  },
  data() {
    return {
      scroller: scrollama(),
      width: 500,
      height: 300,
      step: 0,
      progress: 0,
      top
    };
  },
  created() {
    const { scroller, onEnter, onProgress, onExit, $nextTick } = this;
    // make sure that steps are in the dom before setting up scrollama
    $nextTick(() => {
      scroller
        .setup({
          step: ".scroll-container article .step",
          offset: 0.75,
          progress: true,
          debug: true
        })
        .onStepEnter(onEnter)
        .onStepProgress(onProgress)
        .onStepExit(onExit);
    });
  },
  computed: {
    item() {
      const { step, top } = this;
      return step === 0
        ? {
            color: "orange",
            quote: "INFOVIS",
            fontSize: 6
          }
        : top[step - 1];
    }
  },
  methods: {
    resize(el) {
      const bounds = el.getBoundingClientRect();
      this.width = bounds.width;
      this.height = bounds.height;
      this.scroller.resize();
    },
    onEnter(step) {
      console.log("enter", step.index);
      this.step = step.index;
    },
    onProgress(step) {
      console.log("progress", step.index + step.progress);
      this.step = step.index;
      this.progress = step.progress;
    },
    onExit(step) {
      console.log("exit", step.index);
      this.step = step.index;
    }
  }
};
</script>

<style scoped>
.scroll-container {
  position: relative;
  /* display: flex;
  flex-direction: column; */
}

figure {
  position: sticky;
  top: 0;
  width: 100%;
  height: 100vh;
  background: green;
  z-index: -1;
}
svg {
  display: block;
}

article {
  margin-top: -100vh;
  hyphens: auto;
}

article .step {
  /* background: rgba(255, 255, 255, 0.5); */
  height: auto;
  min-height: 100vh;
}

article .step .inner {
  background: white;
  padding: 2rem;
  margin-bottom: 70vh;

  /* height: 100vh; */
}

article .step .inner h2 {
  margin-bottom: 0;
}
article .step .inner h4 {
  color: gray;
  font-variation-settings: "wght" 300, "slnt" -10;
  margin-bottom: 2rem;
}

a.button {
  display: block;
  text-align: center;
  max-width: 220px;
  background: black;
  padding: 0.5rem;
  margin-top: 1rem;

  text-decoration: none;
  color: white;
  text-transform: uppercase;
  transition: font-weight 0.4s, background 0.4s;
}

a.button:hover {
  font-weight: 900;
  background: blue;
  /* font-variation-settings: "wght" 900, "slnt" -10; */
}

@media screen and (min-width: 640px) {
  figure {
    width: 50%;
    margin-left: auto;
  }
  article .step {
    max-width: 50%;
  }
  article .step .inner {
    margin-bottom: 4rem;
  }
}
</style>
