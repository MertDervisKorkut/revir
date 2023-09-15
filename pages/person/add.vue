<template>
  <v-container>
    <v-card>
      <v-card-title class="justify-center"
        >Personel Sağlık Kontrol Kaydı</v-card-title
      >
      <v-card-text>
        <v-row class="mt-2 justify-center" dense>
          <v-col cols="12" sm="12" md="8" lg="6" xl="6">
            <v-autocomplete
              v-model="adSoyad"
              :items="kisilerDB"
              outlined
              dense
              label="Ad Soyad"
              hide-details
            ></v-autocomplete>
          </v-col>
          <v-col>
            <v-select
              @change="hesaplaTarih"
              v-model="tehlikeDurumu"
              :items="['Tehlikeli', 'Az Tehlikeli']"
              dense
              outlined
              label="Tehlike Durumu"
              hide-details
            ></v-select>
          </v-col>
          <v-col>
            <v-text-field
              label="Başlangıç Tarihi"
              v-model="startDate"
              hide-details
              readonly
              dense
              outlined
            ></v-text-field>
          </v-col>
          <v-col>
            <v-text-field
              label="Bitiş Tarihi"
              v-model="endDate"
              hide-details
              readonly
              dense
              outlined
            ></v-text-field>
          </v-col>
          <v-col cols="12">
            <v-file-input
              v-model="files"
              multiple
              counter
              label="Dosya Ekle"
              prepend-icon=""
              prepend-inner-icon="mdi-paperclip"
              outlined
              dense
              hide-details
            >
              <template v-slot:selection="{ index, text }">
                <v-chip
                  v-if="index < 3"
                  color="deep-purple accent-4"
                  dark
                  label
                  small
                >
                  {{ text }}
                </v-chip>

                <span v-else-if="index === 3" class="text-overline mx-2">
                  +{{ files.length - 3 }} Dosya
                </span>
              </template>
            </v-file-input>
          </v-col>
          <v-col cols="12">
            <v-textarea
              v-model="description"
              hide-details
              dense
              outlined
            ></v-textarea>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions class="justify-center">
        <v-btn color="success" @click="kaydet">Kaydet</v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      startDate: new Date().toLocaleDateString().split(".").reverse().join("-"),
      endDate: null,
      adSoyad: "",
      description: "",
      kisilerDB: ["Mert", "Ahmet", "Nil", "Tuana"],
      tehlikeDurumu: "Tehlikeli",
      files: [],
    };
  },
  created() {
    this.getPersons();
  },
  methods: {
    getPersons() {
      this.$axios
        .get("/person/get-all")
        .then((res) => {
          console.info("gelen veri", res.data);
          this.kisilerDB = res.data.map((e) => e.name + " " + e.surname);
        })
        .catch((err) => {
          console.error(err);
        });
    },
    kaydet() {
      let veri = {
        startDate: this.startDate,
        endDate: this.endDate,
        nameSurname: this.adSoyad,
        description: this.description,
      };
      console.log(veri);

      // Verileri sıfırla
      this.startDate = new Date()
        .toLocaleDateString()
        .split(".")
        .reverse()
        .join("-");
      this.endDate = null;
      this.adSoyad = "";
      this.description = "";
    },
    hesaplaTarih() {
      // Tarih hesaplama bölümü
      this.endDate = new Date(
        new Date(this.startDate).getFullYear() +
          (this.tehlikeDurumu === "Tehlikeli" ? 1 : 2),
        new Date(this.startDate).getMonth(),
        new Date(this.startDate).getDate()
      )
        .toLocaleDateString()
        .split(".")
        .reverse()
        .join("-");
    },
  },
};
</script>
