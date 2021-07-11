<template>
  <breeze-authenticated-layout>
    <template #header>
      <breeze-heading>Edit User - <span v-once>{{ form.name }}</span></breeze-heading>
    </template>

    <div v-show="form.hasErrors">
      <div class="text-red-600 font-medium">
        Please make the following corrects
      </div>
    </div>

    <div v-show="form.wasSuccesful">
      <div class="text-green-600 font-medium">
        Profile updated.
      </div>
    </div>

    <div v-show="form.recentlySuccesful">
      <div class="text-green-600 font-medium">
        Profile was updated.
      </div>
    </div>


    <form @submit.prevent="submit">
      <div class="mb-4">
        <breeze-label for="name" value="Name"></breeze-label>
        <breeze-input
          type="text"
          id="name"
          class="mt-1 block w-1/2"
          v-model="form.name"
          required
          autofocus
          :error="form.errors.name"
        ></breeze-input>
        <breeze-input-error :message="form.errors.name"></breeze-input-error>
      </div>
      <div class="mb-4">
        <breeze-label for="email" value="Email"></breeze-label>
        <breeze-input
          type="text"
          id="email"
          class="mt-1 block w-1/2"
          v-model="form.email"
          :error="form.errors.email"
        ></breeze-input>
        <breeze-input-error :message="form.errors.email"></breeze-input-error>
      </div>

      <div class="mb-4">
        <breeze-label for="image" value="Image"></breeze-label>
        <!-- buttonをクリックしたらinputがメソッドでチェンジする refで参照してbuttonのクリックに合わせてイベントを発火させる。 -->
        <input type="file" class="hidden" ref="photo" @change="updatePhotoPreview">
        <!-- 初期値 -->
        <div class="mt-2" v-show="!photoPreview && user.image">
          <img :src="currentImage()" class="reounded-full h-20 w-20 object-cover">
        </div>
        <!-- プレビュー -->
        <div class="mt-2" v-show="photoPreview">
           <span
            class="block rounded-full w-20 h-20"
            :style=" 'background-size: cover; background-repeat: no-repeat; background-position: center center; background-image: url(\'' + photoPreview + '\');'"
          >
          </span>
        </div>
        <!-- エラー -->
        <breeze-input-error :message="form.errors.image"></breeze-input-error>
        <!-- button -->
        <breeze-button class="mt-2 mr-2" type="button" @click.prevent="selectNewPhoto">Select a New Photo</breeze-button>
      </div>

      <!-- submit -->
      <div class="flex items-center justify-end">
        <breeze-reset-button @click="resetForm">Rest</breeze-reset-button>
        <breeze-button :loading="form.processing">Update</breeze-button>
      </div>
    </form>

  </breeze-authenticated-layout>
</template>

<script>
import { ref } from 'vue';
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated";
import BreezeLink from "@/Components/AnchorLink";
import BreezeInput from "@/Components/Input";
import BreezeLabel from "@/Components/Label";
import BreezeInputError from "@/Components/InputError";
import BreezeButton from "@/Components/Button";
import BreezeResetButton from "@/Components/ResetButton";
import BreezeSelect from "@/Components/Select";
import BreezeHeading from "@/Components/Heading";
import { useForm } from "@inertiajs/inertia-vue3";

export default {
  components: {
    BreezeAuthenticatedLayout,
    BreezeLink,
    BreezeInput,
    BreezeLabel,
    BreezeInputError,
    BreezeButton,
    BreezeResetButton,
    BreezeHeading,
    BreezeSelect,
  },
  props: {
    user: Object,
  },
  setup(props) {

    const form = useForm({
      _method: 'PUT', 
      name: props.user.name,
      email: props.user.email,
      image: props.user.image,
    });

    // submit
    const submit = () => {
      if(photo.value){
        form.image = photo.value.files[0];
      }
      form.post(route("update-profile"), {
        preserveScroll: (page) => Object.keys(page.props.errors).length,
      });
    };

    // form reset
    const resetForm = () => {
      form.clearErrors();
      form.reset();
    };

    // アイコン　初期値
    const currentImage = () => {
      return '/storage/' + props.user.image;
    }

    // テンプレート参照
    // https://v3.vuejs.org/guide/composition-api-template-refs.html#usage-with-jsx
    const photo = ref(null)
    const selectNewPhoto = () => {
      photo.value.click();
    }

    // 画像がアップロードされた時
    const photoPreview = ref(null)
    const updatePhotoPreview = () => {
      const reader = new FileReader();
      reader.onload = (e) => {
        photoPreview.value = e.target.result;
      };
      reader.readAsDataURL(photo.value.files[0]);
    }

    return {
      form,
      submit,
      resetForm,
      currentImage,
      photo,
      selectNewPhoto,
      updatePhotoPreview,
      photoPreview
    };
  },
};
</script>
