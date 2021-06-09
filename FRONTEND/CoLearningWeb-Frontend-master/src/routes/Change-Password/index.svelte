 -<script context="module">
    export async function preload(page, session) {
    //   if (session.user) {
    //     this.redirect(302, `/`)
    //   }
    } 
 </script>




<script >
  

    // import { onMount } from 'svelte';
    import { goto, stores } from "@sapper/app";
    // import {validateCp, validateNp} from '@lib/validation.js'
    import ListErrors from '../_components/ListErrors.svelte';
    import UpdateField from '../_components/UpdateField.svelte'
    import * as api from 'api.js';
    
    // import {validateRequired, validateEmail} from "@lib/validation";
    
    

    const { session } = stores();
    // const dispatch = createEventDispatcher()

    let errors = null;
     errors = 'Messages are displayed here';


    let currentpassword = ''
    let newpassword = ''
    let confirmpassword =''
    // let valid = false
    // let fullName = '';
    let messageType

   

$: current_passwordValid  = currentpassword
$ :passwordValid = newpassword
$ :passwordconfirm_password = passwordValid === confirmpassword

// $ :formIsValid = current_passwordValid && passwordValid && confirm_password

 async function submit(event) {
     
 try {
   
    const Data = {
        current_password:currentpassword,
        new_password:newpassword
    };

    const res =  api.put ( "user/change_password",Data

    ,$session.user.access_token 
    );
    if (res.status >= 400) {
        errors = "warning"
        return errors = res.errors
    }
   
      
      currentpassword = ''
      newpassword = ''
      errors = "success"
      
    
}catch(err){
      console.log(err)
      errors = "Occurred error!!"
}

 }
     
// function handleKeyDown(event) {
//     if (formIsValid && event.keyCode === 13) {
//       submit()
//     }
//   }
   
  
   
           
 </script>

<style>
/* .desc-parent{
    margin: 12px 0 0 0;
}
.desc-text{
    margin: 0 0 5px 0;
}
.red-text{
    color: red; 
} */
</style>
<!-- <svelte:window on:keydown={handleKeyDown}/> -->
<svelte:head>
	<title>Change Password</title>
</svelte:head>

<div class="Change-Password-page">
    <div class="container page">
		<div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Change Password</h1>

                <ListErrors {errors}/>
            <!-- {#if errors}
            <ListErrors errors={errors} messageType={messageType}/>
            {/if} -->

                <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue ={current_passwordValid}
                            iconClass="person-outline"
                            descText="Current password"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={passwordValid}
                            iconClass="person-outline"
                            descText="New Password"
                            inputType="text"
                        />
                    </fieldset>
                    <fieldset class="form-group">
                        <UpdateField
                            bind:updateValue={passwordconfirm_password}
                            iconClass="person-outline"
                            descText="Confirm password"
                            inputType="text"
                        />
                        
                    <button class="btn btn-md btn-primary pull-xs-right"> 
                          Changed Password
					</button>
                </form>
            </div>
        </div>
    </div>
</div>