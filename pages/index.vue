<template>
  <div class="flex justify-center items-center h-screen">
    <div class="w-1/2">
      <h1 class="text-center text-4xl font-bold mb-20">
        Create product review with images in Nuxt.js
      </h1>
      <div class="flex justify-center mt-4">
        <div class="w-1/2 mr-20">
          <div class="flex flex-wrap justify-center">
            <div class="w-full px-3 mb-6 h-24">
              <label
                class="
                  block
                  uppercase
                  tracking-wide
                  text-gray-700 text-xl
                  font-bold
                  mb-4
                "
                for="grid-first-name"
              >
                Comments
              </label>
              <div class="shadow-md rounded px-4 pt-3 pb-4 mb-4">
                <p
                  class="
                    text-center text-l
                    font-bold
                    mb-2
                    border-2 border-gray-200
                  "
                >
                  Image Here!!!
                </p>

                <label
                  class="block uppercase text-gray-700 text-sm font-bold mb-2"
                  for="grid-first-name"
                >
                  Review
                </label>
                <input
                  class="
                    appearance-none
                    block
                    w-full
                    bg-gray-100
                    text-gray-800
                    border border-gray-200
                    rounded
                    py-1
                    px-2
                    leading-tight
                    focus:outline-none focus:bg-white focus:border-gray-500
                  "
                  id="grid-first-name"
                  type="text"
                  readonly
                  value="I love this product"
                />
              </div>

              <div v-if="preview" class="shadow-md rounded px-4 pt-3 pb-4 mb-4">
                <img
                  :src="image"
                  class="
                    w-full
                    rounded-lg
                    mb-2
                    border-2 border-gray-200
                    size-64
                    mx-auto
                    shadow-md
                    hover:shadow-lg hover:border-gray-500
                  "
                />

                <label
                  class="
                    block
                    uppercase
                    tracking-wide
                    text-gray-700 text-sm
                    font-bold
                    mb-2
                  "
                  for="grid-first-name"
                >
                  Review
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
                    py-1
                    px-2
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
        </div>

        <div class="w-1/2">
          <form
            @submit.prevent="submit"
            class="bg-white shadow-md rounded px-6 pt-6 pb-4 mb-4"
          >
            <div class="mb-4">
              <label
                class="block text-gray-700 text-xl font-bold mb-2"
                for="description"
              >
                Comment <span class="text-red-500">*</span>
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
            <div class="mb-4 mt-4">
              <label
                class="block text-gray-700 text-xl font-bold mb-2"
                for="image"
              >
                Image <span class="text-red-500">*</span>
              </label>

              <button
                class="
                  bg-blue-500
                  hover:bg-blue-700
                  text-white
                  font-bold
                  px-2
                  rounded
                  focus:outline-none focus:shadow-outline
                  mb-2
                  py-1
                "
                type="button"
                @click="openCloudinaryWidget"
              >
                Upload
              </button>
              <p v-if="uploaded" class="text-green-500 text-sm italic mb-1">
                Image uploaded!
              </p>
              <button
                class="
                  bg-blue-500
                  hover:bg-blue-700
                  text-white
                  font-bold
                  py-1
                  px-4
                  rounded
                  focus:outline-none focus:shadow-outline
                "
                type="submit"
              >
                Submit
              </button>
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
