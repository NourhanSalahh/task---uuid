<template>
  <div lwc:if={audienceOptions.data}>
   <!-- <lightning-formatted-text>  </lightning-formatted-text> -->
 
     <!-- <img alt="Vue logo" src="./assets/logo.png" style="align-items: center;" /> -->
     <HelloWorld msg="Welcome to Your Delivery App" style="text-align: center;" />
 
     <div class="map-container" >
       <GMapMap
         :center="center"
         :zoom="zoomLevel"
         map-type-id="terrain"
         style="width: 100%; height: 100%; position: absolute; top: 0; left: 0;"
         :options="{
           zoomControl: true,
           mapTypeControl: true,
           scaleControl: true,
           streetViewControl: true,
           rotateControl: true,
           fullscreenControl: true
         }"
       >
         <GMapMarker
           v-if="center.lat !== 0 && center.lng !== 0"
           :position="center"
           :draggable="true"
           :clickable="true"
           @dragend="onMarkerDragEnd"
         />
         
       </GMapMap>
       
     </div>
 
     
 
     <!-- <div class="button-container">
       <button @click="returnToExactLocation" class="btn btn-primary">
         Return to Exact Location
       </button>
     </div> -->
   </div>
 </template>
 
 <script>
 import HelloWorld from './components/HelloWorld.vue';
 import { ref } from 'vue';
 export default {
   name: 'App',
   components: {
     HelloWorld,
   },
 
   setup() {
     // Initialize center using localStorage or default values
     const center = ref({
       lat: 30.05709752372307, // Default to 0 for latitude
       lng: 31.224597740173355, // Default to 0 for longitude
     });
 
     const zoomLevel = ref(20);
 
    
 
    //  const fetchUserLocation = () => {
    //    if ('geolocation' in navigator) {
    //      return new Promise((resolve, reject) => {
    //        navigator.geolocation.getCurrentPosition(
    //          (position) => {
    //            const lat = position.coords.latitude;
    //            const lng = position.coords.longitude;
    //            const newCenter = { lat, lng };
    //            zoomLevel.value = 20;
    //            resolve(newCenter);
    //          },
    //          (error) => {
    //            reject(error);
    //          }
    //        );
    //      });
    //    } else {
    //      console.error("Geolocation is not supported in this browser.");
    //      return null;
    //    }
    //  };
 
     const onMarkerDragEnd = (event) => {
       const newLocation = event.latLng;
       center.value = newLocation;
       saveLocationToLocalStorage(newLocation);
     };
 
    //  const saveLocationToLocalStorage = (location) => {
    //    const locationJSON = JSON.stringify(location);
    //    localStorage.setItem('pinLocation', locationJSON);
    //  };

    const saveLocationToLocalStorage = async (location) => {
      return new Promise((resolve) => {
        const locationJSON = JSON.stringify(location);
        localStorage.setItem('pinLocation', locationJSON);
        resolve();
      });
    };

 
    //  const returnToExactLocation = () => {
    //    fetchUserLocation()
    //      .then((newCenter) => {
    //        center.value = newCenter;
    //        saveLocationToLocalStorage(center.value);
    //      })
    //      .catch((error) => {
    //        console.error("Error getting user location: " + error.message);
    //      });
    //  };
 
    //  onMounted(() => {
    //    fetchUserLocation();
    //  });
 
 
     return {
       center,
       zoomLevel,
      //  returnToExactLocation,
       onMarkerDragEnd,
      
     };
   },
 };
 </script>
 
 <style scoped>
 .map-container {
   width: 100%;
   height: 20rem;
 }
 
 .search-container {
   margin: 10px;
 }
 
 .button-container {
   text-align: center;
   margin: 10px;
 }
 </style>
 
 
 <style scoped>
 .map-container {
   width: 100%;
   height: 20rem;
 }
 
 .search-container {
   margin: 10px;
 }
 
 .button-container {
   text-align: center;
   margin: 10px;
 }
 .dark-mode {
     background-color: #333; /* Change this color to your preferred dark background color */
     color: #fff; /* Change text color to contrast with the dark background */
   }
 
 </style>
 
 