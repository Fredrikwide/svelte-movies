<script>
  //svetle-lifecycle-methods
  import { onMount } from 'svelte';
  import { fetchMovies } from '../api';
  //config
  import { IMAGE_BASE_URL, BACKDROP_SIZE } from '../config';
  // components
  import Hero from '../components/Hero.svelte';
  import Search from '../components/Search.svelte';
  import Grid from '../components/Grid.svelte';
  import Thumb from '../components/Thumb.svelte';
  import LoadMoreButton from '../components/LoadMoreButton.svelte';
  import Spinner from './../components/Spinner.svelte';

  //variables
  let movies = { movies: [] };
  let isLoading;
  let searchTerm = '';
  let error;

  //functions
  const handleFetchMovies = async (loadMore, searchTerm) => {
    try {
      isLoading = true;
      error = false;
      movies = await fetchMovies(movies, loadMore, searchTerm);
      console.log(movies);
    } catch (err) {
      error = true;
    }
    isLoading = false;
  };

  const handleSearch = event => {
    console.log(event);
  };

  //life-cycle
  onMount(async () => {
    handleFetchMovies(false, searchTerm);
  });
</script>

{#if error}
  <p>Something went wrong...</p>
{:else}
  {#if movies.heroImage && !searchTerm}
    <Hero
      image={`${IMAGE_BASE_URL}${BACKDROP_SIZE}${movies.heroImage.backdrop_path}`}
      title={movies.heroImage.original_title}
      text={movies.heroImage.overview} />
  {/if}
{/if}

<Search on:search={handleSearch} />
<Grid />
<Thumb />
<LoadMoreButton />
<Spinner />

<style>
</style>
