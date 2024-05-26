<template>
  <div class="container-fluid">
    <div class="row">
      <div class="d-flex justify-content-end mt-1">
        <nuxt-link to="/" class="btn btn-primary btn-lg px-5">KEMBALI</nuxt-link>
      </div>
      <div class="col-lg-12">
        <h2 class="text-center my-4"></h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Mau baca apa hari ini?" />
          </form>
        </div>

        <div class="row">
          <div v-for="(buku, i) in books" :key="i" class="col-lg-2">
            <nuxt-link :to="`buku/${buku.id}`">
              <div class="card mb-3">
                <div class="card-body">
                  <img :src="buku.cover" class="cover" alt="cover" />
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const keyword = ref("");
const books = ref([]);

const getBooks = async () => {
  const { data, error } = await supabase.from("buku").select(`*, kategori(*)`).ilike("judul", `%${keyword.value}%`);
  if (data) books.value = data;
};

const countBook = async () => {
  const { data, count } = await supabase.from("buku").select("*", { count: "exact" });
};

onMounted(() => {
  getBooks();
  countBook();
});
</script>

<style scoped>
.card-body {
  width: 100%;
  height: 20 rem;
  padding: 0;
  box-shadow: 1px 1px 10px #403610 !important;
}
.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0;
}
.btn-warning {
  box-shadow: 1px 1px 10px #403610 !important;
}
</style>
