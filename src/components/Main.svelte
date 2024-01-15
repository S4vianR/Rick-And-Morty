<!-- Typescript -->
<script lang="ts">
  import { afterUpdate, onMount } from "svelte";

  type Personajes = {
    id: number;
    name: string;
    status: string;
    species: string;
    origin: {
      name: string;
    };
    image: string;
  };

  let currentPage: number = 1;
  let selectedSpecies: string = "";
  let name: string = "";
  let selectedStatus: string = "";
  let api: string = `https://rickandmortyapi.com/api/character/?&page=${currentPage}`;
  let data: { results: Personajes[] } = { results: [] };

  const fetchData = async () => {
    try {
      const res = await fetch(api);
      data = await res.json();
    } catch (error) {
      console.error(error);
    }
  };

  onMount(async () => {
    try {
      await fetchData();
    } catch (error) {
      console.error(error);
    }
  });

  const handleAtrasClick = async () => {
    currentPage = currentPage > 1 ? currentPage - 1 : currentPage;
    api = `https://rickandmortyapi.com/api/character/?page=${currentPage}&name=${name}&species=${selectedSpecies}&status=${selectedStatus}`;

    try {
      await fetchData();
    } catch (error) {
      console.error(error);
    }
  };

  const handleSiguienteClick = async () => {
    currentPage++;
    api = `https://rickandmortyapi.com/api/character/?page=${currentPage}&name=${name}&species=${selectedSpecies}&status=${selectedStatus}`;

    try {
      await fetchData();
    } catch (error) {
      console.error(error);
    }
  };

  const handleSearch = async () => {
    currentPage = 1;
    let searchInput = document.getElementById(
      "searchInput"
    ) as HTMLInputElement;
    name = searchInput.value;
    api = `https://rickandmortyapi.com/api/character/?page=${currentPage}&name=${name}&species=${selectedSpecies}&status=${selectedStatus}`;

    try {
      await fetchData();
    } catch (error) {
      console.error(error);
    }
  };

  const handleGenderChange = async () => {
    currentPage = 1;
    let species = document.getElementById("species") as HTMLSelectElement;
    selectedSpecies = species.value;
    api = `https://rickandmortyapi.com/api/character/?page=${currentPage}&name=${name}&species=${selectedSpecies}&status=${selectedStatus}`;

    try {
      await fetchData();
    } catch (error) {
      console.error(error);
    }
  };

  const handleStatusrChange = async () => {
    currentPage = 1;
    let status = document.getElementById("status") as HTMLSelectElement;
    selectedStatus = status.value;
    api = `https://rickandmortyapi.com/api/character/?page=${currentPage}&name=${name}&species=${selectedSpecies}&status=${selectedStatus}`;

    try {
      await fetchData();
    } catch (error) {
      console.error(error);
    }
  };
</script>

<!-- HTML -->
<section class="w-full flex flex-row justify-between items-center">
  <div class="px-5">
    <h3 class="text-2xl font-bold">Filtros:</h3>
  </div>
  <div class="px-5">
    <h4 class="text-lg font-bold">
      Página: {currentPage}
    </h4>
  </div>
  <div class="px-5 flex flex-row gap-5">
    <button
      on:click={handleAtrasClick}
      class="rounded-md p-2 text-white/80 border-2 border-white/10 hover:bg-slate-500/80 hover:border-white/0"
    >
      Atras
    </button>
    <button
      on:click={handleSiguienteClick}
      class="rounded-md p-2 text-white/80 border-2 border-white/10 hover:bg-slate-500/80 hover:border-white/0"
    >
      Siguiente
    </button>
  </div>
</section>
<section class="flex flex-col gap-5 justify-center items-center p-5">
  <div class="w-full border-2 border-white/20">
    <div
      class="w-full flex flex-row justify-between items-center gap-5 p-5"
    >
      <div class="w-fit flex flex-row gap-3 justify-start items-center">
        <label for="searchInput">Nombre:</label>
        <input
          type="text"
          id="searchInput"
          placeholder="Escribe el nombre del personaje"
          class="w-64 rounded-lg p-2"
          on:keypress={(e) => {
            if (e.key === "Enter") {
              handleSearch();
            }
          }}
        />
        <button
          class="border-2 border-white/20 rounded-lg p-1 hover:bg-slate-400/40 hover:border-white/0"
          on:click={handleSearch}
        >
          Buscar
        </button>
      </div>
      <div class="w-fit flex flex-row gap-3 justify-start items-center">
        <label for="species">Especie:</label>
        <select
          name="species"
          id="species"
          class="w-64 rounded-lg p-2 border-2 border-white/20 hover:cursor-pointer"
          on:change={handleGenderChange}
        >
          <option value="">Todos</option>
          <option value="human">Humanos</option>
          <option value="alien">Aliens</option>
        </select>
      </div>
      <div class="w-fit flex flex-row gap-3 justify-start items-center">
        <label for="species">Estado:</label>
        <select
          name="status"
          id="status"
          class="w-64 rounded-lg p-2 border-2 border-white/20 hover:cursor-pointer"
          on:change={handleStatusrChange}
        >
          <option value="alive">Vivo</option>
          <option value="dead">Fallecido</option>
          <option value="unkown">Desconocido</option>
        </select>
      </div>
    </div>
  </div>
  <ul class="grid grid-cols-7 gap-5">
    {#each data.results as i (i.id)}
      <li
        class="flex flex-col justify-center items-center border-2 border-white/50 p-2 rounded-sm"
      >
        <h4 class="text-lg">{i.name}</h4>
        <p>Species: {i.species}</p>
        <p>Origin: {i.origin.name}</p>
        <img
          src={i.image}
          alt={i.name}
          class="w-48 aspect-square self-center rounded-3xl"
        />
      </li>
    {/each}
  </ul>
</section>
