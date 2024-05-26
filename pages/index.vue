<template>
  <div class="container-fluid">
    <div class="row my-5">
      <div class="col-lg-6">
        <nuxt-link to="/pengunjung/tambah">
          <div class="card bg-pengunjung rounded-5">
            <div class="card-body">
              <h2>Pengunjung</h2>
            </div>
          </div>
        </nuxt-link>
      </div>

      <div class="col-lg-6">
        <nuxt-link to="/buku">
          <div class="card bg-buku rounded-5">
            <div class="card-body">
              <h2>Cari buku</h2>
            </div>
          </div>
        </nuxt-link>
      </div>
    </div>
  </div>
  <div class="statistik rounded-5">
    <h2 class="pt-4 ps-3 fw-bold">STATISTIK</h2>
  </div>
  <div class="row my-5">
    <div class="col-lg-6">
      <div class="card bg-warning rounded-5">
        <div class="card-body text">
          <h2 class="ps-5">{{ visitor }}</h2>
          <h3 class="pt-5">Pengunjung</h3>
        </div>
      </div>
    </div>
    <div class="col-lg-6">
      <div class="card bg-success rounded-5 ms-3">
        <div class="card-body text">
          <h2 class="ps-5">{{ hitungBuku }}</h2>
          <h3 class="pt-5">Buku</h3>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
const supabase = useSupabaseClient();
const visitor = ref(0);
const hitungBuku = ref(0);

const getHitung = async () => {
  const { data, count } = await supabase.from("pengunjung").select("*", { count: "exact" });
  if (data) visitor.value = count;
};
const getHitungbook = async () => {
  const { data, count } = await supabase.from("buku").select("*", { count: "exact" });
  if (data) hitungBuku.value = count;
};

onMounted(() => {
  getHitung();
  getHitungbook();
});
</script>

<style scoped>
.card {
  height: 250px;
  box-shadow: 1px 1px 10px #424242;
}
.card.bg-pengunjung {
  background-image: url("../assets/img/bg-home-kunjungan (1).jpeg");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
.card.bg-buku {
  background: url("../assets/img/bg-home-cari-buku.jpg") no-repeat center;
  background-size: cover;
}

.text {
  display: flex;
  align-items: center;
  justify-content: left;
}
.text h2,
h1 {
  font-size: 7rem;
}

</style>
