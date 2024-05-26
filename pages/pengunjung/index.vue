<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">RIWAYAT</h2>
        <form @submit.prevent="getPengunjung">
          <div class="my-3">
            <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="filter..." />
          </div>
        </form>

        <table class="table">
          <thead>
            <tr>
              <td>No</td>
              <td>NAMA</td>
              <td>KEANGGOTAAN</td>
              <td>TANGGAL/WAKTU</td>
              <td>KEPERLUAN</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(visitor, i) in visitors" :key="i">
              <td>{{ i + 1 }}.</td>
              <td>{{ visitor.nama }}</td>
              <td>{{ visitor.keanggotaan.nama }}</td>
              <td>{{ visitor.tanggal }},{{ visitor.waktu }}</td>
              <td>{{ visitor.keperluan.nama }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
  <div class="d-flex justify-content-end">
    <nuxt-link to="/" class="btn btn-danger btn-lg px-5">Kembali</nuxt-link>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const visitors = ref([]);
const keyword = ref([]);

const getPengunjung = async () => {
  const { data, error } = await supabase.from("pengunjung").select(`*,keanggotaan(*),keperluan(*)`).ilike("nama", `%${keyword.value}%`).order("id", { ascending: false });
  if (data) visitors.value = data;
  if (error) throw error;
};
const pengunjung = async () => {
  const { data, count } = await supabase.from("pengunjung").select(`*`, { count: "exact" });
};

onMounted(() => {
  getPengunjung();
  pengunjung();
});
</script>
