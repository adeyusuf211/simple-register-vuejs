<template>
  <form @submit.prevent="handleSubmit">
    <label for="name">Name:</label>
    <input type="text" placeholder="type your name" v-model="name" required>

    <label for="email">Email:</label>
    <input type="email" placeholder="type your email" v-model="email" required>

    <label for="password">Password:</label>
    <input type="password" placeholder="type your password" v-model="password" required>
    <span class="error" v-if="passwordError">{{ passwordError }}</span>

    <label for="role">Role:</label>
    <select name="role" id="role" v-model="role">
        <option value="web developer">Web Developer</option>
        <option value="designer">Designer</option>
        <option value="ui ux designer">UI UX Designer</option>
    </select>

    <label for="skill">Add skill (Press ',' for add skill)</label>
    <input type="text" v-model="tempSkill" @keyup="addSkill" placeholder="type your skill">
    <div v-for="skill in skills" :key="skill" class="pill">
        <span @click="deleteSkill(skill)">{{ skill }}</span>
    </div>

    <div class="terms">
        <input type="checkbox" v-model="terms" required>
        <label>Please accept terms and condition</label>
    </div>

    <div class="submit">
        <button>Create Account</button>
    </div>

  </form>
</template>

<script>
export default {
    data() {
        return {
            name: "",
            email: "",
            password: "",
            role: "designer",
            terms: false,
            tempSkill: "",
            skills: [],
            passwordError: "",
            data: []
        }
    },
    methods: {
        addSkill(e) {
            if(e.key === "," && this.tempSkill) {
                if(!this.skills.includes(this.tempSkill)) {
                    this.skills.push(this.tempSkill.replace(',', ''));
                }
                this.tempSkill = "";
            }
        },
        deleteSkill(skill) {
            this.skills = this.skills.filter((item) => {
                return skill !== item;
            })
        },
        handleSubmit() {
            this.passwordError = this.password.length >= 8 ? '' : 'Password length min 8 characters';
            if(!this.passwordError) {
                this.data.push({
                    name: this.name,
                    email: this.email,
                    password: this.password,
                    role: this.role,
                    skills: this.skills,
                })
                alert('Registrasi Berhasil');
                localStorage.setItem('data', JSON.stringify(this.data));
            }
        }
    }
}
</script>

<style>
    form {
        max-width: 420px;
        margin: 30px auto;
        background: white;
        text-align: left;
        padding: 40px;
        border-radius: 10px;
    }
    label {
        color: #aaa;
        display: inline-block;
        margin: 25px 0 5px;
        font-size: 0.6em;
        text-transform: uppercase;
        letter-spacing: 1px;
        font-weight: bold;
    }
    input, select {
        display: block;
        padding: 10px 6px;
        width: 100%;
        box-sizing: border-box;
        border: 1px solid #ddd;
        color: #555;
        outline: none;
    }
    input::placeholder {
        color: #555;
    }
    input[type="checkbox"] {
        display: inline-block;
        width: 16px;
        margin: 0 10px 0 0;
        position: relative;
        top: 2px;
    }
    .pill {
        display: inline-block;
        margin: 20px 10px 0 0;
        background: #eee;
        border-radius: 20px;
        font-size: 12px;
        letter-spacing: 1px;
        font-weight: bold;
        color: #777;
        cursor: pointer;
        padding: 6px 12px;
    }
    button {
        background: crimson;
        color: white;
        font-weight: bold;
        padding: 10px 20px;
        border-radius: 100px;
        outline: none;
        border: none;
        margin: 10px auto;
    }
    .submit {
        text-align: center;
    }
    .error {
        font-size: 10px;
        color: crimson;
        margin-top: 10px;
    }
</style>