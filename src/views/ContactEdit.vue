<script>
import ContactForm from '../components/ContactForm.vue';
import contactService from '../services/contact.service';

export default{
    components:{
        ContactForm,
    },
    props:{
        id: {type: String, required: true},
    },
    data(){
        return {
            contact: null,
            message: "",
        }
    },
    methods:{
        async getContact(id){
            try{
                this.contact = await contactService.get(id);
            }catch(error){
                console.log(error);
                // Chuyển sang trang NotFound đồng thời giữ cho URL không đổi
                this.$router.push({
                    name: "notfound",
                    params: {
                        pathMatch: this.$route.path.split("/").slice(1)
                    },
                    query: this.$route.query,
                    hash: this.$route.hash,
                });
            }
        },

        async updateContact(data){
            try{
                await contactService.update(this.contact._id, data);
                this.message = "Liên hệ được cập nhật thành công.";
            }catch(error){
                console.log(error);
            }
        },

        async deleteContact(){
            if(confirm("Bạn muốn xóa liên hệ này")){
                try{
                    await contactService.delete(this.contact._id);
                    this.$router.push({name: "contactbook"});
                }catch(e){  
                    console.log(e);
                }
            }
        },
    },

    created() {
        this.getContact(this.id);
        this.message = "";    
    },
}
</script>

<template>
    <div v-if="contact" class="page">
        <h4>Hiệu chỉnh liên hệ</h4>
        <ContactForm
            :contact="contact"
            @submit:contact="updateContact"
            @delete:contact="deleteContact"
        />
        <p>{{ message }}</p>
    </div>
</template>