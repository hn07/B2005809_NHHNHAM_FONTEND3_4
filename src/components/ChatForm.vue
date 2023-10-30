<template>
    <div class="chat">
        <div class="chat-header">
            <h3 class="chat-title">Thông tin liên hệ</h3>
        </div>
        <div class="chat-body">
            <div class="chat-message" v-for="field in contactFormSchema" :key="field.name">
                <label for="field.name">{{ field.label }}</label>
                <input type="text" class="chat-input" v-model="contactLocal.field.name" />
                <ErrorMessage name="field.name" class="chat-error-feedback" />
            </div>
        </div>
        <div class="chat-footer">
            <button class="chat-btn chat-btn-primary" style="margin-right: 5px;" @click="submitContact">Lưu</button>
            <button v-if="contactLocal._id" style="margin-right: 5px;" type="button" class="chat-btn chat-btn-danger" @click="deleteContact">Xóa</button>
            <button class="chat-btn chat-btn-info" @click="back">Trở lại</button>
        </div>
    </div>
</template>
<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    emits: ["submit:contact", "delete:contact"],
    props: {
        contact: { type: Object, required: true }
    },
    data() {
        const contactFormSchema = yup.object().shape({
            name: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
            email: yup
                .string()
                .email("E-mail không đúng.")
                .max(50, "E-mail tối đa 50 ký tự."),
            address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự."),
            phone: yup
                .string()
                .matches(
                    /((09|03|07|08|05)+([0-9]{8})\b)/g,
                    "Số điện thoại không hợp lệ."
                ),
        });
        return {
            // Chúng ta sẽ không muốn hiệu chỉnh props, nên tạo biến cục bộ
            // contactLocal để liên kết với các input trên form
            contactLocal: this.contact,
            contactFormSchema,
        };
    },
    methods: {
        submitContact() {
            this.$emit("submit:contact", this.contactLocal);
        },
        deleteContact() {
            this.$emit("delete:contact", this.contactLocal.id);
        },
        back() {
            this.$emit("back:contact", this.$router.push({ name: "contactbook" }));
        },
    },
};
</script>
<style scoped>
@import "@/assets/form.css";
</style>
