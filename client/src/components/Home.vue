<template>
  <div>
    <div class="row">
        <div class="col-4">
            <h3>Upload New Image</h3>
            <div class="form-group" style="margin-top: 20%;">
            <label for="exampleInputPassword1">Image</label>
            <input id="newImage" type="file" class="form-control" placeholder="Choose Image">
            </div>
            <button class="btn btn-primary" @click="createImage">Submit</button>
        </div>
        <div class="col-8 d-flex flex-wrap" v-if="isImagesExist">
            <div class="card" style="width: 18rem;" v-for="(image, index) in images" :key="index">
            <img class="card-img-top" :src="image.url" alt="Card image cap" >
            <div class="card-body">
                <h5 class="card-title">{{ image.user.name }}</h5>
                <p class="card-text">{{ image.user.email }}</p>
                <a :href="image.url" class="btn btn-primary">See Image</a>
            </div>
            </div>
        </div>
    </div>    
  </div>
</template>


<script>
import axios from 'axios'

export default {
    data () {
        return {
            images: [],
            isImagesExist: false
        }
    },

    methods: {
        createImage () {                       
            let image = document.getElementById('newImage').files[0]
            console.log(image);
            let payload = new FormData()

            payload.append('photo', image)
            console.log(payload);
            

            axios.post('http://35.198.212.156/api/image', {
                headers: {
                    authorization: localStorage.getItem('tokenUser')
                }
            }, payload)
            .then(response => {
                console.log(response);
                
            })
            .catch(error => {
                console.log(error);
                
            })
        }
    },

    beforeCreate () {
        axios.get('http://35.198.212.156/api/image', {
            headers: {
                authorization: localStorage.getItem('tokenUser')
            }
        })
        .then( response => {
            console.log(response)
            this.images = response.data.images
            this.isImagesExist = true
        })
        .catch( error => {
            console.log(error);
            
        })
    }
}
</script>

<style>

</style>
