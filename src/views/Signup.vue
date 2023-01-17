<template>
  <div>

    <head>
      <title>Facebook</title>

      <link rel="icon" type="image/x-icon"
        href="https://icons.iconarchive.com/icons/yootheme/social-bookmark/512/social-facebook-box-blue-icon.png" />
      <link rel="stylesheet"
        href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css" />
    </head>

    <body>
      <header>
        <div class="header">
          <li class="sitename"><a href="http://Facebook.com">Facebook</a></li>
          <form id="login_form" @submit.prevent="loginForm" name="LogIn" action="post">
            <div id="error"></div>
            <li>
              Email <br /><input type="text" v-model="login_email" id="e-mail" /><br /><small v-if="login_mailError">{{login_mailError}}</small>
            </li>
            <li>
              Password<br /><input type="password" v-model="login_password" name="pass" class="password" /><br /><small v-if="login_passError">{{login_passError}}</small>
              <a href="">Forgotten account?</a>
            </li>
            <li>
              <input type="submit" name="login" value="Log In" class="b"/>
            </li>
          </form>
        </div>
      </header>
      <div class="wrapper">
        <div class="div1">
          <p>
            <B>Connect with friends and the <br />world around you on
              Facebook.</B>
          </p>
          <br /><br />
          <p>
            <i class="fa fa-newspaper-o" aria-hidden="true"></i>
            <B>See photos and updates</B> from friends in New Feed.
          </p>
          <br /><br />
          <p>
            <i class="fa fa-television" aria-hidden="true"></i>
            <B>Share what's new</B> in your life on your Timeline.
          </p>
          <br /><br />
          <p>
            <i class="fa fa-share-alt-square" aria-hidden="true"></i>
            <B>Find more</B> of what you're looking for with Facebook Search.
          </p>
        </div>
        <div class="div2">
          <h1>Sign Up</h1>
          <p>It's free and always will be.</p>
          <form name="signUp" id="signup" @submit.prevent="isFormValid">
            <li>
              <input type="text" v-model="username" placeholder="User Name" id="username" /><br /><small
                v-if="nameError">{{ nameError }}</small>
            </li>
            <li>
              <input type="text" v-model="email" placeholder="email address" id="email" /><br /><small
                v-if="emailError">{{ emailError }}</small>
            </li>
            <li>
              <input type="password" v-model="password" placeholder="New password" id="pass" /><br /><small
                v-if="passwordError">{{ passwordError }}</small>
            </li>
            <li>
              <input type="password" v-model="confirm_password" placeholder="Confirm password"
                id="confirm_pass" /><br /><small v-if="confirmError">{{ confirmError }}</small>
            </li>
            <li>
              <input type="radio" id="female" name="gender" />Female
              <input type="radio" id="male" name="gender"/>Male
              <br /><small></small>
            </li>
            <li class="terms">
              By clicking Sign Up, you agree to our
              <a href="">Teams. Data Police</a> and <br /><a href="">Cookies Policy </a>you may receive SMS Notification
              from us and <br />can opt out
              any time.
            </li>
            <li><input type="submit" value="Sign Up" class="sub" /></li>
          </form>
        </div>
      </div>
      <footer>
        <p><a href="facebook.com">facebook.com</a></p>
      </footer>
    </body>
  </div>
</template>

<script>
export default {
  name: "sign_up",
  data() {
    return {
      username: "",
      email: "",
      password: "",
      confirm_password: "",
      nameError: "",
      emailError: "",
      passwordError: "",
      confirmError: "",
      userDetails: [],
      login_email: "",
      login_password: "",
      login_passError: "",
      login_mailError: "",
    };
  },
  methods: {
    isFormValid() {
      const emailRegx = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/;
      const passwordRegx = /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).*$/;

      if (!this.username) {
        this.nameError = "Username is required";
      } else {
        this.nameError = "";  
      }

      if (!this.email) {
        this.emailError = "E-mail is required";
      } else if (!emailRegx.test(this.email)) {
        this.emailError = "Enter a valid E-mail";
      } else {
        this.emailError = "";  
      }
      if (!this.password) {
        this.passwordError = "Password is required";
      } else if (!passwordRegx.test(this.password)) {
        this.passwordError = "Enter a valid password";
      } else {
        this.passwordError = "";
        
      }
      if (!this.confirm_password) {
        this.confirmError = "Enter the password again";
      } else if (this.confirm_password != this.password) {
        this.confirmError = "Password should be same";
      } else {
        this.confirmError = "";
        
      }
      if (
        this.username &&
        this.email &&
        this.password &&
        this.confirm_password
      ) {
        const user = {
          userName: this.username,
          email: this.email,
          password: this.password,
        };
        alert("Validations successfully working");
        this.userDetails.push(user);
        localStorage.setItem("user", JSON.stringify(this.userDetails));

        this.username='',
        this.email = '',
        this.password ='',
        this.confirm_password =''

      }
    },
    loginForm() {
      const loginDetails = JSON.parse(localStorage.getItem("user"));
      if (!this.login_email) {
        this.login_mailError = "Email cannot be empty";
      } else {
        this.login_mailError = "";
      }
      if (!this.login_password) {
        this.login_passError = "Password cannot be empty";
      } else {
        this.login_passError = "";
      }
      if (this.login_mailError == "" && this.login_passError == "") {
        for (let i in loginDetails){
        if (this.login_email == loginDetails[i].email) {
          if (this.login_password == loginDetails[i].password) {
            this.$router.push("/Dashboard");
          } else {
            this.login_passError("Password does not match");
          }
        } else {
          this.login_mailError = "User does not exist";
        }
        }
      }
    },
  },
};
</script>

<style scoped>
body {
  text-align: center;
  width: 100%;
  margin: 0 auto;
  padding: 0px;
  font-family: "lucida grande", tahoma, verdana, arial, sans-serif;
  background: linear-gradient(white, #d3d8e8);
}

header {
  width: 100%;
  min-width: 980px;
  background-color: #4c66a4;
}

.header {
  width: 980px;
  margin: 0px auto;
  padding: 0px;
  height: 85px;
}

.header li {
  list-style-type: none;
  float: left;
  text-align: left;
  color: white;
}

.header .sitename {
  margin-top: 25px;
}

.header .sitename a {
  color: white;
  text-decoration: none;
  font-size: 30px;
  font-weight: 900;
}

.header form {
  margin-top: 15px;
  float: right;
}

.header form li {
  font-size: 13px;
  margin-left: 15px;
}

.header form li a {
  color: #a9bcf5;
  text-decoration: none;
}

.e {
  margin-top: 3px;
  margin-bottom: 3px;
  width: 150px;
  border: 1px solid #08298a;
  height: 25px;
  padding-left: 3px;
}

.p {
  margin-top: 3px;
  margin-bottom: 3px;
  width: 150px;
  border: 1px solid #08298a;
  height: 25px;
  padding-left: 30px;
}

.b {
  height: 25px;
  margin-top: 20px;
  background-color: #084b8a;
  color: white;
  border: 1px solid #08298a;
}

.wrapper {
  margin: 0 auto;
  padding: 0px;
  text-align: center;
  width: 990px;
}

.wrapper div {
  float: left;
  font-family: helvetica, arial, sans-serif;
}

.wrapper .div1 {
  margin-top: 30px;
  width: 590px;
  text-align: left;
}

.wrapper .div1 p {
  font-size: 20px;
  font-family: arial;
  font-weight: bold;
  margin: 0px;
  color: #000000;
}

.wrapper .div2 {
  margin-top: 10px;
  width: 390px;
  text-align: left;
}

.wrapper .div2 h1 {
  margin: 0px;
  font-size: 37px;
  color: #2e2e2e;
}

.wrapper .div2 p {
  font-size: 18px;
  color: #2e2e2e;
}

.wrapper .div2 li {
  list-style-type: none;
  margin-top: 10px;
}

#username {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding-left: 10px;
  font-size: 18px;
  border: 1px solid #bdbdbd;
}

#email {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding-left: 10px;
  font-size: 18px;
  border: 1px solid #bdbdbd;
}

.email1 {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding-left: 10px;
  font-size: 18px;
  border: 1px solid #bdbdbd;
}

#pass {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding-left: 10px;
  font-size: 18px;
  border: 1px solid #bdbdbd;
}

#confirm_pass {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding-left: 10px;
  font-size: 18px;
  border: 1px solid #bdbdbd;
}

.wrapper .div2 li select {
  padding: 4px;
  float: left;
}

.wrapper .div2 li a {
  margin-left: 10px;
  width: 150px;
  color: #045fb4;
  text-decoration: none;
  font-size: 11px;
  display: inline-block;
  vertical-align: middle;
  line-height: 15px;
}

.wrapper .div2 li a:hover {
  text-decoration: underline;
}

.wrapper .div2 li {
  color: #2e2e2e;
  font-size: 18px;
}

.wrapper .div2 li.terms {
  color: #424242;
  font-size: 11px;
}

.wrapper .div2 li.terms a {
  display: inline;
  margin: 0px;
  font-size: 11px;
}

.sub {
  width: 205px;
  height: 45px;
  text-align: center;
  font-size: 19px;
  margin-top: 10px;
  margin-bottom: 10px;
  font-family: "Freight Sans Bold", helvetica, arial, sans-serif !important;
  font-weight: bold !important;
  background: linear-gradient(#67ae55, #578843);
  background-color: #69a74e;
  box-shadow: inset 0 1px 1px #a4e388;
  border-color: #3b6e22 #3b6e22 #2c5115;
  border: 1px solid;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  position: relative;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
}

footer {
  margin-top: 47%;
  width: 100%;
  min-width: 980px;
  background-color: #4c66a4;
  text-align: center;
  height: 50px;
  padding-top: 15px;
  padding-bottom: 15px;
}

small {
  color: red;
}
input {
  background-color: white;
  border: #000000;
}
</style>
