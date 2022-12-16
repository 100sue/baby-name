<template>
  <div class="container">
    <h1>My Baby's Name</h1>
    <p>Choose your options and click the " Find Names " buttom below</p>
    <div class="options-container">
      <Option
        v-for="option in optionsArray"
        :key="option.title"
        :option="option"
        :options="options"
      />
      <button class="primary" @click="computeSelectedNames">Find Names</button>
    </div>
    <div class="cards-container">
      <CardName
        v-for="(name, index) in selectedNames"
        :key="name"
        :name="name"
        @remove="() => removeName(index)"
        :index="index"
      />
    </div>
  </div>
</template>


<script setup lang="ts">
import { Gender, Popularity, Length, names } from "./data";

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
}

const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  length: Length.SHORT,
  popularity: Popularity.TRENDY,
});

const computeSelectedNames = () => {
  const filteredNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return true;
      else return name.length === options.length;
    });

  selectedNames.value = filteredNames.map((name) => name.name);
};

const selectedNames = ref<string[]>([]);

const removeName = (index: number) => {
  const filteredNames = [...selectedNames.value];
  filteredNames.splice(index, 1);
  selectedNames.value = filteredNames;
};

const optionsArray = [
  {
    title: "Choose a gender :",
    category: "gender",
    buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY],
  },
  {
    title: "Choose the name's popularity :",
    category: "popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE],
  },
  {
    title: "Choose name's length :",
    category: "length",
    buttons: [Length.SHORT, Length.ALL, Length.LONG],
  },
];
</script>



<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

h1 {
  font-size: 3rem;
 font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif ;
}
p {
  color: rgb(49, 75, 223);
  font-weight: 600;
  font-size: 1.3rem;
  font-style: italic;
}
.options-container {
  background-color: rgb(191, 222, 240);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}

.primary {
  background-color:  rgba(255, 255, 255, 0.25);
    border: 1px solid rgba(255, 255, 255, 0.40);
  color: rgb(27, 60, 138);
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-weight: 700;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
  transition: color 0.3s ease;
}
.primary::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 0%;
    background-color: rgb(27, 60, 138);
    z-index: -1;
    transition: width 0.3s ease;
}
.primary:hover::before {
  width: 100%;
}
.primary:hover {
  color: white;
  background-color: rgb(27, 60, 138);
}
.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
</style>
