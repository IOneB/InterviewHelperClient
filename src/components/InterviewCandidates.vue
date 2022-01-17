<template>
  <q-list bordered class="rounded-borders">
    <q-item-label header
      >Кандидаты
      <q-btn @click="prompt = true" rounded style="float: right"
        >+</q-btn
      ></q-item-label
    >
    <q-separator spaced />
    <q-item v-for="candidate in candidates" :key="candidate.id">
      <q-item-section top class="col-2 gt-sm">
        <q-item-label class="q-mt-sm">{{ candidate.name }}</q-item-label>
      </q-item-section>

      <q-item-section top>
        <q-item-label>
          <span class="text-weight-medium">{{
            new Date(candidate.dateInput).toLocaleDateString()
          }}</span>
        </q-item-label>
      </q-item-section>

      <q-item-section top side>
        <div class="text-grey-8 q-gutter-xs">
          <q-btn size="12px" @click="get" flat dense round icon="edit" />
          <q-btn
            class="gt-xs"
            size="12px"
            @click="deleteCandidate(candidate.id)"
            flat
            dense
            round
            icon="delete"
          />
        </div>
      </q-item-section>
    </q-item>
  </q-list>

  <q-dialog v-model="prompt" persistent>
    <q-card style="min-width: 350px">
      <q-card-section>
        <div class="text-h6">Имя</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        <q-input dense v-model="name" autofocus @keyup.enter="prompt = false" />
      </q-card-section>

      <q-card-actions align="right" class="text-primary">
        <q-btn flat label="Отмена" @click="name = ''" v-close-popup />
        <q-btn flat label="Добавить" @click="add" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import parameters from "../modules/common-parameters";

export default {
  props: ["candidates"],
  data() {
    return {
      prompt: false,
      name: "",
    };
  },
  created() {
    console.log(this.candidates);
  },
  methods: {
    async add() {
      try {
        await fetch(parameters.serverAddress, {
          method: "POST",
          headers: {
            "Content-Type": "application/json;charset=utf-8",
          },
          body: JSON.stringify({
            name: this.name,
            dateInput: new Date(),
          }),
        });
      } catch {
        // empty. cors error
      }

      this.name = "";
      this.$emit("reload");
    },
    async deleteCandidate(id) {
      await fetch(`${parameters.serverAddress}/${id}`, {
        method: "DELETE",
      });

      this.$emit("reload");
    },
  },
};
</script>
