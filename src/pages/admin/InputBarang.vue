<template>
    <q-page padding>
         <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
        <div class="col-auto">
        <div class="left blue"></div>
        </div>
        <div class="col">
          <q-banner inline-actions class="text-blue-grey-14">
              <div class="text-h6">Input Barang</div>
              <div>Input Data Barang</div>
          </q-banner>
        </div>
        </div>
      </div>
    </div>
    <q-card flat>
      <q-card-section class="row">
      <q-form
        @submit="onSubmit"
      class="q-gutter-md col-md-6 col-xs-12"
    >
      <q-input
        filled
        v-model="form.namaBarang"
        label="Nama Barang"
        :rules="[ val => val && val.length > 0 || 'Mohon isi Nama Barang']"
      />
      <q-input
            filled
            v-model="form.harga"
            type="number"
            label="Harga"
            :rules="[ val => val  > 0 || 'Mohon isi Harga ']"
        />
        <q-input
            filled
            v-model="form.stok"
            label="Stok Barang"
            :rules="[ val => val && val.length > 0 || 'Mohon isi Stok Barang']"
        />

     <div class="flex">
       Pilih Rating
        <q-rating
      v-model="form.rating"
      size="2em"
      :max="5"
      class="q-ml-md"
      color="orange"
    />
     </div>

      <q-input
      v-model="form.deskripsi"
      filled
      label="deskripsi"
      type="textarea"
    />
    <q-file accept=".jpg, image/*" color="teal" filled v-model="image" label="Upload Gambar">
        <template v-slot:prepend>
          <q-icon name="cloud_upload" />
        </template>
      </q-file>

      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
    </q-form>
      </q-card-section>
    </q-card>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        namaBarang: null,
        harga: 0,
        stok: 0,
        rating: 0,
        deskripsi: null
      },
      image: null
    }
  },
  methods: {
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.post('barang/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'dataBarang' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  }
}
</script>
<style scoped>
  .left {
    width: 3px;
    height: 100%;
    background-color: rgb(47, 141, 218);
  }
</style>
