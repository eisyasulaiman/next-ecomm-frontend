<script>
    import { PUBLIC_BACKEND_BASE_URL } from '$env/static/public';
    import { goto } from '$app/navigation';

    let formErrors = {}; 

    function postSignUp() {
      const successMessage = "Sign-Up successful!";
      window.alert(successMessage);
      goto('/')
    }

    async function createUser(evt) { // async fx  to handle user creation post sign up

      evt.preventDefault()
      
      console.log('Password', evt.target['password'].value);//prevent default form submission to avoid page refresh
      console.log('Password confirmation', evt.target['password-confirmation'].value);//prevent default form submission to avoid page refresh
      
      if (evt.target['password'].value != evt.target['password-confirmation'].value) {
      formErrors = { ...formErrors, password: { message: 'Password confirmation does not match' } };
      return;
    }
    
    //create an oject with user data from the form
    const userData = {
     name: evt.target['name'].value,
     email: evt.target['email'].value,
     password: evt.target['password'].value,
    //  passwordConfirm: evt.target['password-confirmation'].value
    };

    console.log(userData)
    //sentry d a POST req  to create a new user
    const resp = await fetch(`${PUBLIC_BACKEND_BASE_URL}/users`, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(userData),
    });

    console.log(resp)
//check response status
if (resp.status == 200) {
    goto('/');

     if (resp.success) {
        postSignUp();
      } else {
    
        throw 'Sign up succeeded but authentication failed';
      }
    //   } else {
    //if there's an error, parse the response body as JSON and set formErrors
    //   const res = await resp.json(); TO FIX THE BUG FOR EXISTING USERS
    }
  }


 

  //   if (resp.status == 200) {
  //     const responseData = await resp.json();

  //     if (responseData.success) {
  //       postSignUp();
  //       return;
  //     } else {
  //       throw 'Sign up succeeded but authentication failed';
  //     }
  //   } else {
  //     // If there's a server error, parse the response body as JSON and set formErrors
  //     const res = await resp.json();
  //     console.log(res);
  //     if (res.error && res.error.email === 'already taken') {
  //       formErrors.email = 'Email is already taken';
  //     } else {
  //       console.error(res);
  //       formErrors._error = 'Something went wrong';
  //     }
  //   }
  // }
//         } catch (error) {
//         console.error(error);
//         window.alert('An error occurred: ' + error.message);
//     }
// }


</script>

<h1 class="text-center text-xl font-bold">A click away from posting your first image!</h1>

<div class="flex justify-center items-center mt-8">
    <form on:submit={createUser} class="w-1/3">
        <div class="form-control w-full">
            <label class="label" for="name">
                <span class="label-text">Name</span>
            </label>
            <input
                type="text"
                name="name"
                placeholder="johndoe"
                class="input input-bordered w-full"
            />
            {#if 'name' in formErrors}
                <label class="label" for="name">
                    <span class="label-text-alt text-red-500">{formErrors['name'].message}</span>
                </label>
            {/if}
 </div>
 <div class="form-control w-full">
  <label class="label" for="email">
      <span class="label-text">Email</span>
  </label>
  <input
      type="email"
      name="email"
      placeholder="john@example.com"
      class="input input-bordered w-full"
      required
  />
  {#if 'email' in formErrors}
      <label class="label" for="email">
          <span class="label-text-alt text-red-500">{formErrors['email'].message}</span>
      </label>
  {/if}
</div>

<div class="form-control w-full">
  <label class="label" for="password">
      <span class="label-text">Password</span>
  </label>
  <input
      type="password"
      name="password"
      placeholder=""
      class="input input-bordered w-full"
      required
  />
  {#if 'password' in formErrors}
      <label class="label" for="password">
          <span class="label-text-alt text-red-500">{formErrors['password'].message}</span>
      </label>
  {/if}
</div>

<div class="form-control w-full">
  <label class="label" for="password">
      <span class="label-text">Confirm Password</span>
  </label>
  <input
      type="password"
      name="password-confirmation"
      placeholder=""
      class="input input-bordered w-full"
      required
  />
  {#if 'password' in formErrors}
      <label class="label" for="password">
          <span class="label-text-alt text-red-500">{formErrors['password'].message}</span>
      </label>
  {/if}
</div>

      <div class="form-control w-full mt-4">
        <button class="btn btn-md">Create an Account</button>
      </div>
      <div class="text-center">
        <a class="link-hover italic text-xs" href="/users/login"> Login with existing account instead</a>
    </div>
   </form>
</div>