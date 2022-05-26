<template>
  <div class="flex justify-center items-center h-screen">
    <div class="w-1/2">
      <h1 class="text-center text-4xl font-bold mb-20">
        Create product review with images in Nuxt.js
      </h1>
      <div class="flex justify-center mt-4">
        
        <div v-if="preview" class="w-1/2 mr-20">
          <div class="flex flex-wrap justify-center">
            <!-- <div class="w-full px-3 mb-6 h-24"> -->
            <div class="w-full px-3 mb-6 h-24">
              <label
                class="
                  block
                  uppercase
                  tracking-wide
                  text-gray-700 text-xl
                  font-bold
                  mb-2
                "
                for="grid-first-name"
              >
                Comments
              </label>
              <!-- <img :src="image" class="w-full h-64" /> -->
              <!-- Style the image to be small and centered when displayed -->
              <img :src="image" class="w-full h-64 rounded-lg" />

              <label
                class="
                  block
                  uppercase
                  tracking-wide
                  text-gray-700 text-l
                  font-bold
                  mb-2
                "
                for="grid-first-name"
              >
                Text
              </label>
              <input
                class="
                  appearance-none
                  block
                  w-full
                  bg-gray-200
                  text-gray-700
                  border border-gray-200
                  rounded
                  py-3
                  px-4
                  leading-tight
                  focus:outline-none focus:bg-white focus:border-gray-500
                "
                id="grid-first-name"
                type="text"
                readonly
                v-model="text"
              />
            </div>
          </div>
        </div>

        <div class="w-1/2">
          <form
            @submit.prevent="submit"
            class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4"
          >
            <div class="">
              <label
                class="block text-gray-700 text-xl font-bold mb-2"
                for="description"
              >
                Product Review
              </label>
              <textarea
                class="
                  shadow
                  appearance-none
                  border
                  rounded
                  w-full
                  py-4
                  px-4
                  text-gray-700
                  leading-tight
                  focus:outline-none focus:shadow-outline
                "
                id="description"
                type="text"
                placeholder="Add Review"
                v-model="description"
              ></textarea>
            </div>
            <p v-if="error" class="text-red-500 text-sm italic mb-4">
              Please all fields are required!
            </p>
            <div class="mb-4">
              <label
                class="block text-gray-700 text-xl font-bold mb-2"
                for="image"
              >
                Image
              </label>

              <button
                class="
                  bg-blue-500
                  hover:bg-blue-700
                  text-white
                  font-bold
                  py-2
                  px-4
                  rounded
                  focus:outline-none focus:shadow-outline
                  mb-2
                "
                type="button"
                @click="openCloudinaryWidget"
              >
                Upload Image
              </button>
              <p v-if="uploaded" class="text-green-500 text-sm italic mb-2">
                Image uploaded!
              </p>
              <div class="mb-4 mt-12">
                <button
                  class="
                    bg-blue-500
                    hover:bg-blue-700
                    text-white
                    font-bold
                    py-2
                    px-4
                    rounded
                    focus:outline-none focus:shadow-outline
                  "
                  type="submit"
                >
                  Submit
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      description: "",
      image: "",
      text: "",
      error: false,
      preview: false,
      uploaded: false,
    };
  },

  methods: {
    submit() {
      if (this.description === "" || this.image.length === "") {
        this.preview = false;
        this.error = true;
      } else {
        this.text = this.description;
        this.preview = true;
      }
    },

    createCloudinaryWidget() {
      const newWidget = cloudinary.createUploadWidget(
        {
          cloudName: process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME,
          uploadPreset: process.env.NUXT_ENV_CLOUDINARY_UPLOAD_PRESET,
          multiple: false,
          maxFiles: 1,
          cropping: true,
          croppingAspectRatio: 1,
          croppingCoordinateMode: "face",
          clientAllowedFormats: ["png", "gif", "jpeg"],
        },
        (error, result) => {
          //checking if upload was successfully done!
          if (!error && result && result.event === "success") {
            console.log(result.info);
            //save url to a server database, or preform any other logic here
            this.image = result.info.secure_url;
            this.uploaded = true;
          }
        }
      );
      return newWidget;
    },
    openCloudinaryWidget() {
      const widget = this.createCloudinaryWidget();
      widget.open();
    },
  },
};
</script>
