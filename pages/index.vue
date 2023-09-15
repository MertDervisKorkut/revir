<template>
  <v-container fluid>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6">
        <v-card>
          <v-card-title class="justify-center">Personel Tehlike Durum Sistemi</v-card-title>
          <v-card-text>
            <!-- Kullanıcı adı girişi -->
            <!--v-text-field
              v-model="personelisim"
              label="Personel İsim "
              outlined
              dense
            ></v-text-field-->

          
            
            <!-- Butonları aynı satırda koymak için -->
            <v-row>
              <v-col 
              cols="6">
                <v-btn
                  color="primary"
                  block
                  @click="yenikayit"
                  href="/person/add"

                >
              
                  Yeni Kayıt
                </v-btn>
              </v-col>
              <v-col cols="6">
                <v-btn
                  color="secondary"
                  block
                  @click="sorgula"
                  href="/person/status"
                >
                  Sorgula
                </v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      personelisim: "",
      
    };
  },
  methods: {
    yenikayit() {
      // Kayıt işlemleri burada gerçekleştirilebilir
      console.log("Personel İsim:", this.personelisim);
    },
    sorgula() {
  
    }
  }
};
</script>
