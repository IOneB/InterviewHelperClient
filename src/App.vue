<template>
  <div class="q-pa-md">
    <q-layout view="lhh LpR lff" class="shadow-2 rounded-borders">
      <interview-navigation :subjects="subjects" @activate="activateSection" />
      <q-page-container>
        <q-page class="q-pa-md">
          <div v-for="subject in subjects" :key="subject.id">
            <div
              class="bg-red full-width"
              :subject="subject"
              v-show="subject.id === currentSubject"
              v-if="subject.id === 'Candidates'"
            >
              {{ subject.id === "Candidates" }}
            </div>
            <interview-page
              v-else
              :subject="subject"
              :show="subject.id === currentSubject"
            />
          </div>
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>

<script>
import parameters from "./modules/common-parameters.js";
import interfaceModule from "./modules/interface-template";

import InterviewNavigation from "./components/InterviewNavigation.vue";
import InterviewPage from "./components/InterviewPage.vue";

export default {
  data() {
    return {
      data: "loaded",
      url: parameters.serverAddress,
      subjects: interfaceModule.createSubjects(),
      currentSubject: "",
    };
  },
  async created() {
    // try {
    //   const resp = await fetch(this.url);
    //   this.data = await resp.text();
    // } catch (error) {
    //   this.data = error;
    // }
  },
  methods: {
    activateSection(id) {
      this.currentSubject = id;
    },
    getInfo() {
      const subjects = Object.keys(this.subjects).filter(
        (id) => id !== "Candidates"
      );
      const info = subjects
        .map((id) => {
          const subject = this.subjects[id];
          const points = subject.points
            .map((p) => ({
              grade: p.grade,
              skills: p.skills
                .filter((sk) => sk.value || sk.comment)
                .map((sk) => ({
                  id: sk.id,
                  value: sk.value,
                  comment: sk.comment,
                })),
            }))
            .filter((x) => x.skills.length);

          return {
            id,
            points,
          };
        })
        .filter((s) => s.points.length);

      console.log(info);
    },
  },
  components: {
    InterviewNavigation,
    InterviewPage,
  },
};
</script>
