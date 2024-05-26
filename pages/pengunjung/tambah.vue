<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h3 class="text-center my-4">ISI BUKU KUNJUNGAN</h3>
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <input v-model="form.nama" type="text" class="form-control form-control-lg rounded-5" placeholder="NAMA..." />
          </div>

          <div class="mb-3">
            <select v-model="form.keanggotaan" class="form-control form-control-lg form-select rounded-5">
              <option value="">KEANGGOTAAN</option>
              <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
            </select>
          </div>
          <div class="row" v-if="form.keanggotaan == '3'">
            <div class="mb-3">
              <div class="mb-3">
                <select v-model="form.tingkat" class="form-control form-control-lg form-select rounded-5 mb-2">
                  <option value="">TINGKAT</option>
                  <option value="X">X</option>
                  <option value="XI">XI</option>
                  <option value="XII">XII</option>
                </select>
              </div>

              <div class="mb-3">
                <select v-model="form.jurusan" class="form-control form-control-lg form-select rounded-5 mb-2">
                  <option value="">JURUSAN</option>
                  <option value="PPLG">PPLG</option>
                  <option value="TKJ">TKJ</option>
                  <option value="TSM">TSM</option>
                  <option value="DKV">DKV</option>
                  <option value="TOI">TOI</option>
                </select>
              </div>
              <div class="mb-3">
                <select v-model="form.kelas" class="form-control form-control-lg form-select rounded-5">
                  <option value="">KELAS</option>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                </select>
              </div>
            </div>
          </div>

          <div class="mb-3">
            <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-5">
              <option value="">KEPERLUAN</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
          <div class="d-flex justify-content-between">
            <button type="submit" class="btn bg-primary btn-lg rounded-5 px-5">Kirim</button>
            <nuxt-link to="/" class="btn btn-danger btn-lg px-5">Kembali</nuxt-link>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const members = ref([]);
const objectives = ref([]);

const form = ref({
  nama: "",
  keanggotaan: "",
  tingkat: "",
  jurusan: "",
  kelas: "",
  keperluan: "",
});

const kirimData = async () => {
  // console.log(form.value)
  const { error } = await supabase.from("pengunjung").insert([form.value]);
  if (!error) navigateTo("/pengunjung");
};
const getKeanggotaan = async () => {
  const { data, error } = await supabase.from("keanggotaan").select("*");
  if (data) members.value = data;
};
const getKeperluan = async () => {
  const { data, error } = await supabase.from("keperluan").select("*");
  if (data) objectives.value = data;
};
onMounted(() => {
  getKeanggotaan();
  getKeperluan();
});
</script>

<style scoped>
.bg-primary {
  box-shadow: 1px 1px 10px #403610 !important;
}
.btn-warning {
  box-shadow: 1px 1px 10px #403610 !important;
}
</style>
