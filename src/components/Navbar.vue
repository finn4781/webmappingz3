<script>
import ModalContact from "./subcomponents/ModalContact.vue"
import ModalAbout from "./subcomponents/ModalAbout.vue"
import ModalPrivacy from "./subcomponents/ModalPrivacy.vue"
import ModalTerms from "./subcomponents/ModalTerms.vue"
import ModalFaq from "./subcomponents/ModalFaq.vue"
import OffCanvas from "./subcomponents/OffCanvas.vue"
import ModalFilter from "./subcomponents/ModalFilter.vue"


export default {
  components: {ModalContact, ModalAbout, ModalPrivacy, ModalTerms, ModalFaq, OffCanvas, ModalFilter},
  props: ['title', 'price', 'description', 'image'],
  data() {
    return {
      
    }
  }, 
  methods:{
    applyFilter(data) {
      // console.log('')
      this.$emit('apply-filter', data);
    }
  }

}
</script>


<template>

  <nav class="navbar navbar-expand-lg bg-light" id="navbarSection">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">MyBrandName</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a type="button" class="nav-link" data-bs-toggle="modal" data-bs-target="#contactModal">
              Contact
            </a>
          </li>
          <li class="nav-item">
            <a type="button" class="nav-link" data-bs-toggle="modal" data-bs-target="#modalAbout">
              About
            </a>

          </li>
          <li class="nav-item">
            <a class="nav-link" type="button" data-bs-toggle="modal" data-bs-target="#privacyModal">
              Privacy policy
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link " type="button" data-bs-toggle="modal" data-bs-target="#termsModal">
              Terms of use 
            </a>
          </li>
          <li>
            <a class="nav-link" type="button" data-bs-toggle="modal" data-bs-target="#faqModal">
              FAQ
            </a>
          </li>
        </ul>
       

      </div>
    </div>
  </nav>




  

<ModalContact/>
<ModalAbout/>
<ModalPrivacy/>
<ModalTerms/>
<ModalFaq/>

<ModalFilter @apply-filter="applyFilter($event)"/>

<OffCanvas :title="title" :price="price" :description="description" :image="image"/>



</template>



<style scoped>

</style>