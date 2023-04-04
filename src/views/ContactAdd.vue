<template>       
    <div>
        <h4>Thêm liên hệ</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="addContact"
        />
        <p>{{ message }}</p>
    </div>
</template>

<script>
import ContactForm from '../components/ContactForm.vue';
import contactService from '../services/contact.service';

export default{
    components:{
        ContactForm,
    },

    data(){
        return {
            contact: null,
            message: "",
        }
    },

    methods:{
        async addContact(data){
            try{
                await contactService.create(data);
                this.message = "Liên hệ được thêm thành công.";
            }catch(e){
                console.log(e);
                alert("Có lỗi xảy ra khi thêm liên hệ mới");
            }
        }
    },

    created(){
        this.contact = {
            name: "",
            address: "",
            email: "",
            phone: "",
            favorite: false
        };
    }
}
</script>