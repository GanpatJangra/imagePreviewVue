<template>
    <div class="bg-gray-300 pb-10">
        <div>
            <!-- {{  formData.cropProfilePicture }} -->
            <!-- <img class="h-32 w-32 " :src="formData.cropProfilePicture" alt=""> -->
            <input ref="openModelRef" type="checkbox" id="my-modal-6" class="modal-toggle" />
            <div class="modal modal-bottom sm:modal-middle">
                <div class="modal-box">
                    <vue-cropper ref="cropper" class="!h-[400px]" :aspect-ratio="16 / 16" :scalable="false"
                        :cropBoxResizable="false" :src="formData.profile_image" dragMode="none" preview=".preview" />
                    <div>
                        <input @change="handelImageRangeValue" v-model.number="imageRangeValue" type="range" min="-0.5"
                            max="0.5" step="0.1" class="w-full mt-5 bg-gray-300 rounded-lg focus:outline-none" />
                        <div class="modal-action flex flex-col">
                            <!-- <input type="range" min="0" max="100" value="40" class="range h-5 mt-5" /> -->
                            <div class="justify-between flex">
                                <label for="my-modal-6" class="btn btn-sm btn-error">Close !</label>
                                <button type="button" @click="cropImage" class="btn btn-success btn-sm">Submit</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- {{ imageUrl }} -->
        <div>
            <input ref="openModelViewImageRef" type="checkbox" id="my-modal-200" class="modal-toggle" />
            <div class="modal modal-bottom  sm:modal-middle">
                <div class="modal-box">
                    <div v-if="imageUrl">
                        <img loading="lazy" id="myImage" :src="imageUrl" alt="">
                        <div class="modal-action justify-between">
                            <label for="my-modal-200" class="btn btn-error btn-sm">Close !</label>
                            <button type="button" @click="downloadImage(imageUrl)" class="btn btn-success btn-sm">download
                                Image</button>
                        </div>
                    </div>
                    <div v-else>
                        <!-- <img loading="lazy" id="myImage" :src="imageUrl" alt=""> -->
                        <div class="h-32 flex items-center flex-col capitalize font-medium justify-center ">
                            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
                                <g>
                                    <circle cx="3" cy="12" r="2" fill="currentColor" />
                                    <circle cx="21" cy="12" r="2" fill="currentColor" />
                                    <circle cx="12" cy="21" r="2" fill="currentColor" />
                                    <circle cx="12" cy="3" r="2" fill="currentColor" />
                                    <circle cx="5.64" cy="5.64" r="2" fill="currentColor" />
                                    <circle cx="18.36" cy="18.36" r="2" fill="currentColor" />
                                    <circle cx="5.64" cy="18.36" r="2" fill="currentColor" />
                                    <circle cx="18.36" cy="5.64" r="2" fill="currentColor" />
                                    <animateTransform attributeName="transform" dur="1.5s" repeatCount="indefinite"
                                        type="rotate" values="0 12 12;360 12 12" />
                                </g>
                            </svg>
                            <h1>please wait image is generating ... </h1>
                        </div>
                        <div class="modal-action justify-between">
                            <label for="my-modal-200" class="btn btn-error btn-sm">Close !</label>
                            <button type="button" class="btn btn-success btn-sm">Wait ....</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="bg-[#4406CB] w-full h-64">
            <img loading="lazy" class="rounded-md h-40 mx-auto pt-14" src="@/assets/image/LogoICAI.png" alt="logo">
        </div>
        <div class="-mt-20 md:w-2/3 w-[90%]  bg-white rounded mx-auto ">
            <div class="flex flex-col md:flex-row p-5">
                <div class="md:w-1/2 w-full">
                    <img loading="lazy" class="  h-full " src="@/assets/image/Image_to_show.jpg" alt="">
                </div>
                <div class="md:w-1/2 w-full md:px-10">
                    <div>
                        <div class="form-control w-full max-w-xs">
                            <div>
                                <label class="label">
                                    <span class="label-text font-semibold capitalize ">Profile Image<span
                                            class="text-red-500"> *</span></span>
                                </label>
                                <!-- <input type="file" placeholder="Your Name" class="input input-bordered w-full max-w-xs" /> -->
                                <!-- <input @change="handelFileData" type="file"
                                class="file-input file-input-bordered file-input-primary w-full max-w-xs" /> -->
                                <input @change="handelFileData" type="file" accept="image/*"
                                    class="file-input file-input-bordered file-input-primary w-full max-w-xs" />
                                <!-- <input @change="handelFileData" type="file" class="file-input  file-input-bordered file-input-success w-full max-w-xs" /> -->
                            </div>
                            <div>
                                <label class="label">
                                    <span class="label-text font-semibold capitalize ">name<span class="text-red-500">
                                            *</span></span>
                                </label>
                                <input v-model.trim="formData.name" type="text" placeholder="Your Name"
                                    class="input input-bordered w-full max-w-xs" />
                            </div>
                            <div>
                                <label class="label">
                                    <span class="label-text font-semibold capitalize ">designation<span
                                            class="text-red-500"> *</span></span>
                                </label>
                                <input v-model="formData.designation" type="text" placeholder="Your designation"
                                    class="input input-bordered w-full max-w-xs" />
                            </div>
                            <div>
                                <label class="label">
                                    <span class="label-text font-semibold capitalize">region or branch<span
                                            class="text-red-500"> *</span></span>
                                </label>
                                <input v-model="formData.region_or_branch" type="text" placeholder="Your region or branch"
                                    class="input input-bordered w-full max-w-xs" />
                            </div>
                            <span class="text-red-500" v-for="item in errors" :key="item.id">
                                {{ item }}
                            </span>
                            <button @click="submitForm" :disabled="enableLoading"
                                class="mt-7 space-x-2 btn btn-primary text-white text-base">
                                <svg v-if="enableLoading" xmlns="http://www.w3.org/2000/svg" width="20" height="20"
                                    viewBox="0 0 24 24">
                                    <g>
                                        <circle cx="12" cy="3" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate0" attributeName="r"
                                                begin="0;svgSpinners12DotsScaleRotate2.end-0.5s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="16.5" cy="4.21" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate1" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate0.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="7.5" cy="4.21" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate2" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate4.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="19.79" cy="7.5" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate3" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate1.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="4.21" cy="7.5" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate4" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate6.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="21" cy="12" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate5" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate3.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="3" cy="12" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate6" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate8.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="19.79" cy="16.5" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate7" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate5.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="4.21" cy="16.5" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate8" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotatea.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="16.5" cy="19.79" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotate9" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate7.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="7.5" cy="19.79" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotatea" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotateb.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <circle cx="12" cy="21" r="1" fill="currentColor">
                                            <animate id="svgSpinners12DotsScaleRotateb" attributeName="r"
                                                begin="svgSpinners12DotsScaleRotate9.begin+0.1s" calcMode="spline"
                                                dur="0.6s" keySplines=".27,.42,.37,.99;.53,0,.61,.73" values="1;2;1" />
                                        </circle>
                                        <animateTransform attributeName="transform" dur="6s" repeatCount="indefinite"
                                            type="rotate" values="360 12 12;0 12 12" />
                                    </g>
                                </svg>
                                <span>Submit</span></button>
                        </div>
                    </div>
                </div>
                <div>
                </div>
            </div>
        </div>
        <!-- <footer class="text-center mt-5 font-medium"><a href="http://Eminenceinfotech.com">Eminenceinfotech.com</a></footer> -->
        <footer class="text-center mt-5 font-medium"><a href="https://Eminenceinfotech.com" target="_blank"
                rel="noopener noreferrer">Eminenceinfotech.com</a></footer>
    </div>
</template>
<script setup>
import { useHead } from "@vueuse/head"
import VueCropper from 'vue-cropperjs';
import 'cropperjs/dist/cropper.css';
import { ref, computed, reactive, watch } from 'vue';
import axios from 'axios';
const formData = ref({
    profile_image: null,
    name: '',
    designation: '',
    region_or_branch: '',
    cropProfilePicture: null
})
const imageRangeValue = ref(0.1)
const errors = ref([])
const openModelRef = ref(null)
const openModelViewImageRef = ref(null)
let imageUrl = ref('');
// const cropImg = ref(null)
const enableLoading = ref(false)
const cropper = ref(null)
function cropImage() {
    // get image data for post processing, e.g. upload or setting image src
    formData.value.cropProfilePicture = cropper.value.getCroppedCanvas().toDataURL();
    openModelRef.value.click()
}
function handelImageRangeValue() {
    // console.log("🚀 ~ file: app.vue:199 ~ handelImageRangeValue ~ imageRangeValue.value:", imageRangeValue.value)
    cropper.value.relativeZoom(imageRangeValue.value);
    // zoom(percent) {
    //     this.$refs.cropper.relativeZoom(percent);
    //   },
}


// Set the canvas data with the custom size

// function zooooooooom() {
//     alert(
//         'faksldfjla'
//     )
// }


// const customSize = {
//   left: 0,       // The offset left of the canvas
//   top: 0,        // The offset top of the canvas
//   width: 500,    // The new width of the canvas
//   height: 400    // The new height of the canvas
// }
function handelFileData(e) {
    // openModelRef.value.click()
    // return
    errors.value = []
    const file = e.target.files[0];
    if (file.type.indexOf('image/') === -1) {
        errors.value.push('Please select an image file');
        return
    }
    if (typeof FileReader === 'function') {
        const reader = new FileReader();
        reader.onload = (event) => {
            formData.value.profile_image = event.target.result;
            // rebuild cropperjs with the updated source
            cropper.value.replace(event.target.result);
        };
        reader.readAsDataURL(file);
        // cropper.value.setCanvasData(customSize);
        openModelRef.value.click()
    } else {
        alert('Sorry, FileReader API not supported');
    }
    // formData.value.profile_image = event.target.files[0];
}
async function submitForm() {
    // Check if required fields are empty
    errors.value = []
    if (!formData.value.name || !formData.value.designation || !formData.value.region_or_branch || !formData.value.profile_image) {
        return errors.value.push('Please fill in all required fields');
    }
    // Check if designation exceeds 20 characters
    if (formData.value.designation.length > 20) {
        return errors.value.push('Designation should not exceed 20 characters');
    }
    openModelViewImageRef.value.click()

    let form = new FormData();
    const imageData = await formData.value.cropProfilePicture; // Replace with your actual Base64-encoded image data
    enableLoading.value = true
    // Create a Blob from the Base64 data
    const blob = await base64ToBlob(imageData)
    // Append the image blob to the form data
    form.append('name', formData.value.name);
    form.append('designation', formData.value.designation);
    form.append('region_or_branch', formData.value.region_or_branch);
    form.append('profile_image', blob, `${formData.value.name || 'profile'}.png`);
    // 'image' is the field name, 'image.png' is the desired file name
    // form.append('profile_image', formData.value.cropProfilePicture);
    let headers = {
        'Content-Type': 'multipart/form-data'
    };
    axios.post('https://apifincobox.sentientdigital.in/api/v1/gib/image', form, { headers })
        .then(resp => {
            imageUrl.value = resp.data.results.image_url;
            enableLoading.value = false
            // downloadImage()
        })
        .catch(error => {
            console.error("Error:", error);
        });
}
// function DownLoadImage() {
//   if (imageUrl.value) {
//       const link = document.createElement('a');
//       link.href = imageUrl.value;
//       // link.target = '_blank';
//       link.download = '';
//       link.click();
//       // link.download = 'Banner-image.png';
//       // link.textContent = 'Download Image';
//   }
// }
// Function to convert Base64 to Blob
function base64ToBlob(base64Data) {
    const parts = base64Data.split(';base64,');
    const contentType = parts[0].split(':')[1];
    const byteCharacters = atob(parts[1]);
    const byteArrays = [];
    for (let offset = 0; offset < byteCharacters.length; offset += 1024) {
        const slice = byteCharacters.slice(offset, offset + 1024);
        const byteNumbers = new Array(slice.length);
        for (let i = 0; i < slice.length; i++) {
            byteNumbers[i] = slice.charCodeAt(i);
        }
        const byteArray = new Uint8Array(byteNumbers);
        byteArrays.push(byteArray);
    }
    return new Blob(byteArrays, { type: contentType });
}
// async function downloadImage(url) {
//   try {
//     const imageSrc = url;
// } catch (error) {
//     console.error('Error downloading image:', error);
//   }
// }
// const liveImageUrl = computed(() => imageUrl.value)
async function downloadImage(imageURL) {
    try {
        const response = await axios.get(imageURL, {
            responseType: 'blob',
        })
        const blob = response.data;
        const url = URL.createObjectURL(blob);
        Object.assign(document.createElement('a'), { href: url, download: `${formData.value.name}` }).click();
        URL.revokeObjectURL(url);
    } catch (error) {
        console.error('Error downloading image:', error);
    }
}

const siteData = reactive({
    title: `The Institute of Chartered Accountants of India `,
    description: `The Institute of Chartered Accountants of India, Committee on MSME & Startup, ICAI`,
})

useHead({
    // Can be static or computed
    title: () => siteData.title,
    meta: [
        {
            name: `description`,
            content: () => siteData.description,

        },
    ],
})
console.log('v12')
</script>
<style>
.cropper-view-box {
    display: block;
    height: 100%;
    outline: 1px solid #39f;
    border-radius: 100%;
    outline-color: rgba(51, 153, 255, 0.75);
    overflow: hidden;
    width: 100%;
}

.cropper-line.line-e,
.cropper-line.line-n,
.cropper-line.line-w,
.cropper-line.line-s {
    display: none !important;
}

.cropper-point.point-sw,
.cropper-point.point-nw,
.cropper-point.point-ne,
.cropper-point.point-w,
.cropper-point.point-e,
.cropper-point.point-s,
.cropper-point.point-n {
    display: none !important;
}

.cropper-point.point-se {
    bottom: -3px;
    cursor: nwse-resize;
    height: 10px;
    opacity: 1;
    right: -3px;
    width: 10px;
}

.cropper-face {
    background-color: transparent;
    left: 0;
    top: 0;
}</style>