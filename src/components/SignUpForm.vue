<template>
<div class="form">
    <form @submit.prevent="handleSubmit">
        <div>
            <label for="email">Email:</label>
            <input type="email" id="email" v-model="formData.email" required>
        </div>
        <div>
            <label for="password">Password:</label>
            <input type="password" id="password" v-model="formData.password" required pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[\W]).{8,}">
        </div>
        <div>
            <label for="role">Role:</label>
            <select id="role" v-model="formData.role" required>
                <option disabled value="">Please select one</option>
                <option>Web Developer</option>
                <option>Web Designer</option>
            </select>
        </div>
        <div>
            <label>Skills:</label>
            <input type="text" v-model="skillInput" @keyup.enter="addSkill" @keypress="checkForComma" />
            <ul class="skills">
                <li v-for="(skill, index) in formData.skills" :key="index">
                    {{ skill }}
                    <svg xmlns="http://www.w3.org/2000/svg" height="0.8rem" viewBox="0 0 448 512" @click="removeSkill(index)">
                        <path d="M135.2 17.7L128 32H32C14.3 32 0 46.3 0 64S14.3 96 32 96H416c17.7 0 32-14.3 32-32s-14.3-32-32-32H320l-7.2-14.3C307.4 6.8 296.3 0 284.2 0H163.8c-12.1 0-23.2 6.8-28.6 17.7zM416 128H32L53.2 467c1.6 25.3 22.6 45 47.9 45H346.9c25.3 0 46.3-19.7 47.9-45L416 128z" /></svg>
                </li>
            </ul>
        </div>
        <div class="term-condition">
            <input type="checkbox" id="terms" v-model="formData.terms" required>
            <label for="terms">Accept Terms & Conditions</label>
        </div>
        <button type="submit">Create an Account</button>
    </form>
    <div v-if="submittedData">
        <h3>Submitted Data:</h3>
        <p>{{ submittedData }}</p>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            formData: {
                email: '',
                password: '',
                role: '',
                skills: [],
                terms: false,
            },
            skillInput: '',
            submittedData: null,
        };
    },
    methods: {
        addSkill() {
            if (this.skillInput.trim() !== '') {
                this.formData.skills.push(this.skillInput.trim());
                this.skillInput = '';
            }
        },
        checkForComma(event) {
            if (event.key === ',' || event.key === 'Enter') {
                event.preventDefault();
                this.addSkill();
            }
        },
        removeSkill(index) {
            this.formData.skills.splice(index, 1);
        },
        handleSubmit() {
            this.submittedData = this.formData;
        },
    },
};
</script>

<style scoped>
form {
    max-width: 400px;
    margin: auto;
    padding: 50px 30px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

form div {
    margin-bottom: 20px;
}

label {
    text-align: start;
    display: block;
    margin-bottom: 5px;
    color: #040404;
    font-weight: 700;
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
}

.term-condition label {
    margin-left: 2px;
    margin-top: 7px;
}
</style>
