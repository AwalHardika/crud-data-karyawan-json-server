<script>
  import FormInput from "./Components/FormInput.svelte";
  import ListKaryawan from "./Components/ListKaryawan.svelte";


</script>

<main class={`w-screen min-h-screen flex flex-col gap-2 bg-slate-100 items-center p-6`}>
  <h1 class={`text-orange-500`}>Input Data Karyawan</h1>
  <FormInput/>
  <ListKaryawan/>
</main>