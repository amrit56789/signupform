<template>
<div class="form">
    <form @submit.prevent="handleSubmit">
        <div class="form-data">
            <label for="email">Email:</label>
            <input type="email" id="email" v-model.lazy="formData.email" class="form-control" />
            <p v-if="formErrors.email" class="error">{{ formErrors.email }}</p>
        </div>

        <div class="form-data">
            <label for="password">Password:</label>
            <input type="password" id="password" v-model.lazy="formData.password" class="form-control" />
            <p v-if="formErrors.password" class="error">{{ formErrors.password }}</p>
        </div>

        <div class="form-data">
            <label for="role">Role:</label>
            <select id="role" v-model="formData.role" class="form-control">
                <option disabled value="">Please select one</option>
                <option>Web Developer</option>
                <option>Web Designer</option>
            </select>
            <p v-if="formErrors.role" class="error">{{ formErrors.role }}</p>
        </div>

        <div class="form-data">
            <label>Skills:</label>
            <input type="text" v-model="skillInput" @keyup.enter="addSkill" @keypress="checkForComma" class="form-control" />
            <p v-if="formErrors.skills" class="error">{{ formErrors.skills }}</p>
            <ul class="skills">
                <li v-for="(skill, index) in formData.skills" :key="index">
                    {{ skill }}
                    <DeleteIcon @click="removeSkill(index)" />
                </li>
            </ul>
        </div>

        <div class="term-condition">
            <div class="term-checkbox">
                <input type="checkbox" id="terms" v-model="formData.terms" />
                <label for="terms">Accept Terms & Conditions</label>
            </div>
            <p v-if="formErrors.terms" class="error">{{ formErrors.terms }}</p>
        </div>

        <button type="submit" class="submit-btn">Create an Account</button>
    </form>
    <div v-if="submittedData">
        <h3>Submitted Data:</h3>
        <p>{{ submittedData }}</p>
    </div>
</div>
</template>

<script>
import DeleteIcon from './DeleteIcon.vue'

export default {
    components: {
        DeleteIcon
    },
    data() {
        return {
            formData: {
                email: "",
                password: "",
                role: "",
                skills: [],
                terms: false,
            },
            skillInput: "",
            submittedData: null,
            formErrors: {},
        };
    },
    methods: {
        addSkill() {
            if (this.skillInput.trim() !== "") {
                this.formData.skills.push(this.skillInput.trim());
                this.skillInput = "";
            }
        },
        checkForComma(event) {
            if (event.key === "," || event.key === "Enter") {
                event.preventDefault();
                this.addSkill();
            }
        },
        removeSkill(index) {
            this.formData.skills.splice(index, 1);
        },
        handleSubmit() {
            this.formErrors = this.validateFormData();
            if (Object.keys(this.formErrors).length === 0) {
                this.submittedData = JSON.stringify(this.formData, null, 2);
            }
        },
        validateFormData() {
            const errors = {};
            if (!this.formData.email) {
                errors.email = "Email is required";
            }
            if (!this.formData.password) {
                errors.password = "Password is required";
            } else {
                const passwordPattern = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*(),.?":{}|<>])[A-Za-z\d!@#$%^&*(),.?":{}|<>]{8,}$/;
                if (!passwordPattern.test(this.formData.password)) {
                    errors.password = "Password must be at least 8 characters long, include at least 1 special character, 1 number, 1 uppercase, and 1 lowercase letter.";
                }
            }

            if (!this.formData.role) {
                errors.role = "Role is required";
            }
            if (this.formData.skills.length === 0) {
                errors.skills = "At least one skill is required";
            }
            if (!this.formData.terms) {
                errors.terms = "Please accept the terms & conditions";
            }
            return errors;
        }
    },
};
</script>

<style scoped>
form {
    max-width: 400px;
    margin: auto;
    padding: 20px 30px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

form div {
    margin-bottom: 2px;
}

label {
    text-align: start;
    display: block;
    margin-bottom: 5px;
    color: #040404;
    font-weight: 700;
}

div p {
    text-align: start;
    font-size: 0.8rem;
}

.form-data {
    margin-top: 30px;
}

input[type=email],
input[type=password],
select,
input[type=text] {
    width: 100%;
    padding: 8px;
    border-radius: 4px;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid wheat;
    margin: 2px 0;
}

input[type=checkbox] {
    margin-right: 5px;
}

.skills {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
}

.skills li {
    display: flex;
    align-items: center;
    gap: 10px;
}

button[type=submit] {
    background-color: #3b1bbc;
    color: white;
    padding: 13px 40px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-weight: 700;
    font-size: 1rem;
    margin-top: 30px;
    border-radius: 20px;
}

ul {
    list-style-type: none;
    padding-left: 0;
}

li {
    margin-bottom: 10px;
    background-color: #fbd8d8;
    padding: 7px 12px;
    border-radius: 15px;
    color: black;
}

button {
    background-color: rgb(87, 48, 204);
    color: white;
    border: none;
    border-radius: 4px;
    padding: 5px 10px;
    cursor: pointer;
}

.error {
    color: red;
    margin-bottom: 15px;
}

.term-condition {
    display: flex;
    align-content: center;
    flex-direction: column;
}

.term-checkbox {
    display: flex;
}

.term-condition label {
    margin-left: 2px;
    margin-top: 2px;
}
</style>
