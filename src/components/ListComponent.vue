<template>
  <div class="modalList">
    <q-list inset-separator class="listHover" v-for="(test, index) in tests" :key="index">
      <q-item>
        <div class="tightenList">
          <div>
            <q-item-side class="eventTime">{{test.start}} {{test.end}}</q-item-side>
          </div>
          <q-item-main class="eventDetails" :label="test.title"/>
          <q-item-main class="eventDetails noMargin" color="grey" :label="test.details"/>
          <q-item-side right>
            <q-btn flat round dense icon="more_vert" text-color="black" class="iconStyle">
              <q-popover>
                <q-list link>
                  <q-item v-close-overlay>
                    <q-item-main label="Edit" @click.native="editExistingEvent(index)"/>
                  </q-item>
                  <q-item v-close-overlay>
                    <q-item-main label="Delete" @click.native="deleteExistingEvent(index)"/>
                  </q-item>
                </q-list>
              </q-popover>
            </q-btn>
          </q-item-side>
        </div>
      </q-item>
    </q-list>
    <div class="modalBtm">
      <div class="listCountLbl">
        <label>{{lblCountList}}{{totalCount}}</label>
      </div>
    </div>
  </div>
</template>

<script>
import { LocalStorage, SessionStorage } from "quasar";
export default {
  name: "list-component",
  props: ["date", "mth", "year"],
  data: () => {
    return {
      tests: [],
      lblCountList: "No. of Events: ",
      totalCount: "",
      key: ""
    };
  },
  created() {
    this.passDates(this.date, this.mth, this.year);
    this.setTotalCount();
  },
  methods: {
    setTotalCount() {
      this.totalCount = this.tests ? this.tests.length : 0;
    },
    passDates(date, mth, year) {
      var key = `${date}/${mth}/${year}`;
      this.key = key;
      this.tests = LocalStorage.get.item(key);
      this.setTotalCount();
    },
    deleteExistingEvent(index) {
      this.tests.splice(index, 1);
      LocalStorage.set(this.key, this.tests);
      this.setTotalCount();
      this.$emit("updateCalendar");
    },
    editExistingEvent(index) {
      let key = `editEvent`;
      LocalStorage.set(key, this.tests[index]);
      key = `editEventIndex`;
      LocalStorage.set(key, index);
      this.$emit("editEvent");
    }
  }
};
</script>

<style>
.modalDesign {
  padding: 30px;
  width: 50vw;
  height: 80vh;
}

.modalTitle {
  top: 0vh;
  font-size: 20px;
  width: 300px;
}

.fixed {
  position: fixed;
}

.modalList {
  margin-top: 3vh;
  margin-bottom: 3vh;
  overflow: hidden;
  height: 60vh;
  width: 100%;
  overflow-y: scroll;
  border-style: solid;
  border-color: lightgrey;
  padding: 0px;
  border-radius: 8px;
}

.container {
  -ms-overflow-style: none;
}
.container::-webkit-scrollbar {
  display: none;
}

.tightenList {
  margin: 0px;
}

.iconStyle {
  right: 0;
  top: 0;
  position: absolute;
}

.eventTime {
  position: absolute;
  width: 3%;
  padding-top: 0px;
}

.eventDetails {
  padding-left: 3.6vw;
  width: 38vw;
  height: 3%;
  overflow: hidden;
  top: 0px;
}

.noMargin {
  padding-left: 2.95vw;
  color: rgb(173, 169, 169);
}

.listHover {
  padding: 0px;
  margin: 0px;
  height: 8vh;
}

.listHover :hover {
  /* background-color: lightgrey; */
}

.listCountLbl {
  bottom: 4vh;
  position: absolute;
}

.removeBorder {
}

@media only screen and (max-width: 1000px) {
  .eventDetails {
    padding-left: 4.8vw;
  }

  .noMargin {
    padding-left: 3.7vw;
  }
}

@media only screen and (max-width: 766px), (max-height: 500px) {
  .modalList {
    width: 86vw;
    height: 75vh;
  }

  .modalDesign {
    margin: 0px;
  }

  .floatRight {
    right: 3vw;
    position: absolute;
  }
  .listHover :hover {
    background-color: transparent;
  }

  .eventDetails {
    padding-left: 6vw;
    height: 20px;
    width: 70vw;
    font-size: 14px;
    margin: 0px;
  }

  .noMargin {
    padding-left: 4.5vw;
  }

  .eventTime {
    font-size: 14px;
  }

  .addEventBtn {
    width: 80vw;
  }
}

@media only screen and (max-width: 600px) {
  .eventDetails {
    padding-left: 8vw;
  }

  .noMargin {
    padding-left: 6vw;
  }
}

@media only screen and (max-width: 450px) {
  .eventDetails {
    padding-left: 10vw;
  }

  .noMargin {
    padding-left: 7.8vw;
  }
}

@media only screen and (max-width: 350px) {
  .eventDetails {
    padding-left: 16%;
  }

  .noMargin {
    padding-left: 12%;
  }
}
</style>
