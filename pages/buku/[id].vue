<template>
  <div class="container-fluid">
    <div class="row">
      <h3 style="text-align: center">DETAIL</h3>
      <div class="col-lg-2">
        <div class="card mt-5">
          <div class="card-body">
            <img :src="buku.cover" class="cover" alt="cover" />
          </div>
        </div>
      </div>

      <div class="col-lg-4">
        <div class="row">
          <ul class="list-group list-group-flush" style="margin-top: 65px">
            <li class="list-group-item">Judul : {{ buku.judul }}</li>
            <li class="list-group-item">
              <span v-if="buku.kategori">Kategori : {{ buku.kategori.nama }}</span>
              <span v-else>loading...</span>
            </li>
            <li class="list-group-item">Penulis : {{ buku.penulis }}</li>
            <li class="list-group-item">Penerbit : {{ buku.penerbit }}</li>
            <li class="list-group-item">Tahun Terbit : {{ buku.tahun_trbit }}</li>
            <li class="list-group-item">Deskripsi : {{ buku.deskripsi }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>

  <nuxt-link to="../buku" class="btn btn-lg rounded-3 px-5 bg-primary text-black" style="float: right"> Kembali </nuxt-link>
</template>

<script setup>
const Supabase = useSupabaseClient();

const route = useRoute();
const buku = ref([]);

const getBookById = async () => {
  const { data, error } = await Supabase.from("buku").select(`*, kategori(*)`).eq(`id`, route.params.id);
  if (data) buku.value = data[0];
};

onMounted(() => {
  getBookById();
});
</script>

<style scoped>
img {
  width: 100%;
}
</style>
