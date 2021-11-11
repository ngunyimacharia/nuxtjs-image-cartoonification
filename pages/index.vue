<template>
  <div class="bg-white">
    <div class="max-w-7xl mx-auto py-16 px-4 sm:py-24 sm:px-6 lg:px-8">
      <div class="text-center">
        <p
          class="
            mt-1
            text-4xl
            font-extrabold
            text-gray-900
            sm:text-5xl sm:tracking-tight
            lg:text-6xl
          "
        >
          Image cartoonification
        </p>
        <p class="max-w-xl mt-5 mx-auto text-xl text-gray-500">
          This tool will cartoonify your images for you. Upload your image and
          see it cartoonified for you.
        </p>
      </div>
    </div>

    <div v-if="cloudinaryImage" class="text-center">
      <cld-image
        :public-id="cloudinaryImage.public_id"
        width="500"
        height="400"
        crop="pad"
        background="black"
        class="mx-auto"
      >
        <cld-transformation effect="cartoonify" />
      </cld-image>
      <a
        v-if="cloudinaryImage"
        target="_blank"
        :href="imageUrl"
        class="
          my-5
          inline-flex
          items-center
          px-4
          py-2
          border border-transparent
          text-sm
          font-medium
          rounded-md
          text-indigo-700
          bg-indigo-100
          hover:bg-indigo-200
          focus:outline-none
          focus:ring-2
          focus:ring-offset-2
          focus:ring-indigo-500
        "
      >
        Download
      </a>
    </div>

    <div
      class="
        bg-white
        px-4
        py-5
        border
        rounded-lg
        border-gray-200
        sm:px-6
        w-2/3
        mx-auto
        text-center
      "
    >
      <form @submit.prevent="upload">
        <input
          class="
            my-5
            block
            w-full
            border border-gray-100
            rounded-md
            shadow-sm
            py-2
            px-3
            focus:outline-none focus:ring-indigo-500 focus:border-indigo-500
            sm:text-sm
          "
          type="file"
          name="file"
          @change="handleFile"
        />
        <p v-if="uploading" class="text-center text-gray-700">Uploading...</p>
        <button
          v-else-if="image"
          type="submit"
          class="
            block
            mx-auto
            items-center
            px-4
            py-2
            border border-transparent
            text-sm
            font-medium
            rounded-md
            shadow-sm
            text-white
            bg-indigo-600
            hover:bg-indigo-700
            focus:outline-none
            focus:ring-2
            focus:ring-offset-2
            focus:ring-indigo-500
          "
        >
          Convert
        </button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploading: false,
      image: null,
      cloudinaryImage: {
        asset_id: "2bbe272f2615690bc5ae8f89beba2be9",
        public_id: "nuxtjs-image-cartoonification/mjdqsl487qiqvnhouofa",
        version: 1636603656,
        version_id: "3c1980bd63536d878a957865ce912292",
        signature: "0e0316918e36b564798352df20738a1393e54d46",
        width: 4000,
        height: 6000,
        format: "jpg",
        resource_type: "image",
        created_at: "2021-11-11T04:07:36Z",
        tags: [],
        bytes: 2395779,
        type: "upload",
        etag: "47ae18f89e6d103071ed6c7b6b6a631c",
        placeholder: false,
        url: "http://res.cloudinary.com/hackit-africa/image/upload/v1636603656/nuxtjs-image-cartoonification/mjdqsl487qiqvnhouofa.jpg",
        secure_url:
          "https://res.cloudinary.com/hackit-africa/image/upload/v1636603656/nuxtjs-image-cartoonification/mjdqsl487qiqvnhouofa.jpg",
        access_mode: "public",
      },
    };
  },
  computed: {
    imageUrl() {
      return this.$cloudinary.image.url(this.cloudinaryImage.public_id, {
        effect: "cartoonify",
      });
    },
  },
  methods: {
    async handleFile(e) {
      this.image = e.target.files[0];
    },
    async readData(f) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
    },
    async upload() {
      this.uploading = true;
      const imageData = await this.readData(this.image);
      this.cloudinaryImage = await this.$cloudinary.upload(imageData, {
        upload_preset: "default-preset",
        folder: "nuxtjs-image-cartoonification",
      });
      console.log(this.cloudinaryImage);
      this.uploading = false;
    },
  },
};
</script>