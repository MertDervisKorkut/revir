User
<template>
  <v-container>
    <v-row>
      <v-col cols="5">
        <v-autocomplete
          v-model="personName"
          :items="persons"
          dense
          outlined
          hide-details
        ></v-autocomplete>
      </v-col>
      <v-col cols="1" class="px-0 mx-0">
        <v-btn color="primary" fab small><v-icon>mdi-check</v-icon></v-btn>
      </v-col>
    </v-row>
    <v-row dense>
      <v-col cols="11">
        <v-card>
          <v-card-title class="d-flex justify-center">{{
            person.nameSurname
          }}</v-card-title>
          <v-card-text>
            <v-simple-table>
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-center">Başlangıç Zamanı</th>
                    <th class="text-center">Bitiş Zamanı</th>
                    <th class="text-center">Açıklama</th>
                    <th class="text-center">Dökümanlar</th>
                  </tr>
                </thead>
                <tbody>
                  <tr align="center">
                    <td>{{ person.startDate }}</td>
                    <td>{{ person.endDate }}</td>
                    <td>{{ person.description }}</td>
                    <td>
                      <v-btn color="info"><v-icon>mdi-download</v-icon></v-btn>
                    </td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
          </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="1">
        <v-list>
          <v-list-item>
            <v-btn block @click="sortUpcomingPersons" color="purple"
              ><v-icon>mdi-sort</v-icon></v-btn
            >
          </v-list-item>
          <v-list-item
            class="my-1"
            v-for="(personel, personelIndex) of upcomingPersons"
            :key="personelIndex"
          >
            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
                <v-icon
                  :color="checkDate(personel.endDate, personel.isChecked)"
                  large
                  v-bind="attrs"
                  v-on="on"
                  >mdi-calendar</v-icon
                >
              </template>
              <span>{{ personel.endDate }}</span>
            </v-tooltip>

            <v-spacer></v-spacer>

            <v-tooltip bottom>
              <template v-slot:activator="{ on, attrs }">
                <v-icon color="blue" large v-bind="attrs" v-on="on"
                  >mdi-account</v-icon
                >
              </template>
              <span>{{ personel.nameSurname }}</span>
            </v-tooltip>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    personName: null,
    persons: ["Mert", "Onur", "Ali"],
    person: {
      nameSurname: "Onur",
      startDate: "2023-09-04",
      endDate: "2024-09-04",
      description: "Falan Filan",
    },
    upcomingPersons: [
      {
        nameSurname: "Onur",
        startDate: "2023-04-15",
        endDate: "2023-09-29",
        isChecked: false,
      },
      {
        nameSurname: "Mert",
        startDate: "2022-08-10",
        endDate: "2023-09-14",
        isChecked: false,
      },
      {
        nameSurname: "Nil",
        startDate: "2023-08-30",
        endDate: "2023-09-19",
        isChecked: false,
      },
      {
        nameSurname: "Nuriye",
        startDate: "2022-09-11",
        endDate: "2023-09-30",
        isChecked: false,
      },

      {
        nameSurname: "Tuana",
        startDate: "2023-05-20",
        endDate: "2023-09-13",
        isChecked: false,
      },
      {
        nameSurname: "Berkay",
        startDate: "2022-07-10",
        endDate: "2023-08-30",
        isChecked: false,
      },
      {
        nameSurname: "Derviş",
        startDate: "2023-08-10",
        endDate: "2023-12-19",
        isChecked: false,
      },
      {
        nameSurname: "Açelya",
        startDate: "2023-09-01",
        endDate: "2023-09-20",
        isChecked: false,
      },
      
    ],
    sortDesc: false,
  }),
  methods: {
    sortUpcomingPersons() {
      this.sortDesc = !this.sortDesc;

      this.upcomingPersons = this.upcomingPersons.sort((a, b) =>
        this.sortDesc ? a.endDate < b.endDate : a.endDate > b.endDate
      );
      console.log(this.upcomingPersons);
    },
    dateControl(year, month) {
      // Belirtilen yıl ve ay için bir tarih nesnesi oluşturun
      let date = new Date(year, month - 1, 1);

      // Ayın son gününü bulun (şubatta 28 olacak şekilde)
      date.setMonth(date.getMonth() + 1);
      date.setDate(date.getDate() - 1);

      // Son günün gün numarasını döndürün
      return date.getDate();
    },

    checkDate(endDate, isChecked) {
      let color;
      let _endDate = new Date(endDate);
      let date = new Date();

      let _endMonthDate = this.dateControl(
        _endDate.getFullYear(),
        _endDate.getMonth() + 1
      );

      let thisMontDate = this.dateControl(
        date.getFullYear(),
        date.getMonth() + 1
      );

      /*
      console.log(
        "-----------",
        _endDate.getMonth() + 1,
        ". ay",
        _endMonthDate,
        "gün"
      );
      console.log("songün", _endDate.getDate());

      console.log(
        "-----------",
        date.getMonth() + 1,
        ". ay",
        thisMontDate,
        "gün"
      );
      console.log("bugün ayın " + date.getDate());
      */

      let remainingDay = 0;
      let flag = false;
      if (_endDate.getMonth() == date.getMonth()) {
        remainingDay = Math.abs(date.getDate() - _endDate.getDate());
        flag = date.getDate() > _endDate.getDate();
      } else {
        remainingDay = thisMontDate - date.getDate() + _endDate.getDate();
        flag= _endDate.getMonth() < date.getMonth()
      }

      if (flag) {
        color = "error";
      } else if (remainingDay > 15) {
        color = "primary";
      } else if (remainingDay <= 15 && remainingDay > 5) {
        color = "success";
      } else if (remainingDay <= 5) {
        color = "warning";
      }
      return color;
    },
  },
};
</script>
<style></style>
