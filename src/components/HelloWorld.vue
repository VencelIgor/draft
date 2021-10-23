<template>
  <v-container>
    <v-radio-group v-model="activeFirstPlayer" row disabled>
      <v-radio label="Player 1" :value="true"></v-radio>
      <v-radio label="Player 2" :value="false"></v-radio>
    </v-radio-group>
    <div class="row" v-for="(row, rowIndex) in characters" :key="rowIndex">
      <div
        class="character"
        v-for="(character, name) in row"
        :key="name"
        @click="takeCharacter(rowIndex, name)"
      >
        <img
          :class="{ picked: character.isPicked, banned: character.isBanned }"
          :src="require(`../assets/characters/${name}.jpg`)"
          :alt="name"
        />
      </div>
    </div>
    <div class="row picked-row">
      First Player:
      <div
        class="character"
        v-for="character in picked.firstPlayer"
        :key="character"
      >
        <img
          :src="require(`../assets/characters/${character}.jpg`)"
          :alt="character"
        />
      </div>
    </div>
    <div class="row picked-row">
      Second Player:
      <div
        class="character"
        v-for="character in picked.secondPlayer"
        :key="character"
      >
        <img
          :src="require(`../assets/characters/${character}.jpg`)"
          :alt="character"
        />
      </div>
    </div>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: () => ({
    characters: [
      {
        Casandra: {},
        Siegfried: {},
        Zasalamel: {},
        Nightmare: {},
        Amy: {},
      },
      {
        '2b': {},
        Xianghua: {},
        Maxi: {},
        Sophitia: {},
        Astaroth: {},
        Ivy: {},
        Tira: {},
      },
      {
        Yoshimitsu: {},
        Kilik: {},
        Groh: {},
        Cervantes: {},
      },
      {
        Talim: {},
        Mitsurugi: {},
        Geralt: {},
        Taki: {},
        Azwel: {},
      },
      { Seong: {}, Raphael: {}, Voldo: {} },
    ],
    picked: {
      firstPlayer: [],
      secondPlayer: [],
    },
    activeFirstPlayer: true,
    orderChoice: [
      'ban1',
      'ban2',
      'ban1',
      'ban2',
      'pick1',
      'pick2',
      'pick2',
      'pick1',
      'ban1',
      'ban2',
      'pick2',
      'pick2',
      'pick1',
      'pick1',
      'ban1',
      'ban2',
      'pick1',
      'pick2',
    ],
  }),
  methods: {
    takeCharacter(rowIndex, name) {
      if (!this.characters[rowIndex][name].isBanned && !this.characters[rowIndex][name].isPicked) {
        const currentTurn = this.orderChoice.shift();

        switch (currentTurn) {
          case 'pick1':
            this.$set(this.characters[rowIndex][name], 'isPicked', 'true');
            this.picked.firstPlayer.push(name);
            break;
          case 'pick2':
            this.$set(this.characters[rowIndex][name], 'isPicked', 'true');
            this.picked.secondPlayer.push(name);
            break;
          case 'ban1':
            this.$set(this.characters[rowIndex][name], 'isBanned', 'true');
            break;
          case 'ban2':
            this.$set(this.characters[rowIndex][name], 'isBanned', 'true');
            break;
          default:
            break;
        }
      }
    },
  },
  watch: {
    orderChoice() {
      if (this.orderChoice[0] === 'ban1' || this.orderChoice[0] === 'ban2') {
        this.activeFirstPlayer = true;
      } else {
        this.activeFirstPlayer = false;
      }
    },
  },
  beforeCreate() {},
};
</script>

<style lang="scss">
.row {
  justify-content: center;
}
.character {
  flex-basis: calc(100% / 7);
  flex-grow: 0;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;

  img {
    border: 3px solid transparent;
  }

  .banned {
    border: 3px solid red;
  }

  .picked {
    border: 3px solid green;
  }
}
.picked-row {
  display: flex;
  align-items: center;
}
</style>
