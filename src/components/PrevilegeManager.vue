<template>
  <div>
      <v-dialog v-model="dialog"  width="500">
          <v-card class="pa-2">
              <v-layout column>
                  <v-flex xs12 md6>
                    <v-stepper v-model="e1">
                         <v-stepper-header>
                            <v-stepper-step :complete="e1 > 1" step="1">User Details</v-stepper-step>

                            <v-divider></v-divider>

                            <v-stepper-step :complete="e1 >= 2" step="2">Provide Permissions</v-stepper-step>

                            <v-divider></v-divider>

                            <v-stepper-step step="3">Verify</v-stepper-step>
                        </v-stepper-header>
                         <v-stepper-items>
                                <v-stepper-content step="1">
                                    <v-card class="pa-2" flat>
                                        <v-layout row>
                                            <v-flex >
                                                <v-list>
                                                    <v-list-item>
                                                        <v-list-item-icon >
                                                            <v-chip>
                                                                <v-icon left color="red">mdi-account</v-icon>
                                                                User
                                                            </v-chip>
                                                        </v-list-item-icon>
                                                        <v-list-item-content class="mx-4">
                                                            <v-text-field required v-model="username"
                                                            
                                                            > </v-text-field>
                                                        </v-list-item-content>
                                                    </v-list-item>
                                                    <v-list-item>
                                                        <v-list-item-icon >
                                                            <v-chip>
                                                                <v-icon left color="red">mdi-lock</v-icon>
                                                                Password
                                                            </v-chip>
                                                        </v-list-item-icon>
                                                        <v-list-item-content>
                                                            <v-text-field v-model="password" ref="value"  :type="show1?'password':'text'"
                                                                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                                                                :rules="[
                                                                () => !!password || 'This field is required',
                                                                () => !!password && password.length>=8||'Password must be greater than 8 characters' 
                                                                ]"
                                                                counter
                                                                required
                                                                @click:append="show1= !show1"
                                                            > </v-text-field>
                                                        </v-list-item-content>
                                                    </v-list-item>
                                                    <v-list-item>
                                                        <v-list-item-icon>
                                                            <v-chip>
                                                                <v-icon left color="red">mdi-lock</v-icon>
                                                               Re-Password
                                                            </v-chip>
                                                        </v-list-item-icon>
                                                        <v-list-item-content>
                                                              <v-text-field v-model="password1" ref="value"  :type="show1?'password':'text'"
                                                                :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
                                                                :rules="[
                                                                () => !!password1 || 'This field is required',
                                                                password1 => password1 == password||'Passwords do not match' 
                                                                ]"
                                                                counter
                                                                required
                                                                @click:append="show1= !show1"
                                                            > </v-text-field>
                                                                 
                                                        </v-list-item-content>
                                                    </v-list-item>
                                                    <v-list-item>
                                                       
                                                            <v-list-item-content>
                                                                <v-btn  color="primary" :disabled="everythingOk ==true " @click="e1 = 2"  >
                                                                    Continue      
                                                                </v-btn>
                                                                <v-btn text @click="dialog=false">
                                                                    Cancel
                                                                </v-btn>
                                                            </v-list-item-content>
                                                    </v-list-item>
                                                </v-list>
                                            </v-flex>
                                        </v-layout>
                                    </v-card>
                                </v-stepper-content>
                                <v-stepper-content step="2">
                                    <v-card>
                                        <v-list>
                                                <v-list-item v-for="permission in permissions" :key="permission.name">
                                                    <v-list-item-action>
                                                        <v-checkbox
                                                            v-if="permission.name ==='Admin'"
                                                            v-model="active"
                                                            :value="permission.name"
                                                            color="primary"
                                                            @click.stop="admin"
                                                        ></v-checkbox>
                                                        <v-checkbox
                                                            v-else
                                                            v-model="active"
                                                            :value="permission.name"
                                                            color="primary"
                                                           
                                                        ></v-checkbox>
                                                    </v-list-item-action>

                                                    <v-list-item-content>
                                                        <v-list-item-title>{{permission.name}}</v-list-item-title>
                                                        <v-list-item-subtitle>{{permission.desc}}</v-list-item-subtitle>
                                                    </v-list-item-content>
                                                </v-list-item>
                                                <v-list-item>
                                                       
                                                    <v-list-item-content>
                                                        <v-btn  color="primary" @click="e1 =3" :disabled="hasRole" >
                                                            Continue      
                                                        </v-btn>
                                                        <v-btn text @click="e1 =1">
                                                            Back
                                                        </v-btn>
                                                    </v-list-item-content>
                                                </v-list-item>
                                        </v-list>
                                    </v-card>
                                </v-stepper-content>
                         </v-stepper-items>
                        <v-stepper-content step="3">
                            <v-card>
                                <v-list>
                                    <v-subheader class="display-1 ma-4">{{username}}</v-subheader>
                                    <v-list-item v-for="selected in active" :key="selected" >
                                        <v-chip
                                            class="ma-2"
                                            color="pink"
                                            label
                                            text-color="white"
                                            >
                                            <v-icon left>mdi-label</v-icon>
                                            {{selected}}
                                            </v-chip>
                                    </v-list-item>
                                    <v-list-item>
                                        <v-list-item-content>
                                             <v-btn  color="primary" @click="submit"  >
                                                           Submit     
                                             </v-btn>
                                            <v-btn text @click="e1 =2">
                                                            Back
                                             </v-btn>
                                        </v-list-item-content>
                                    </v-list-item>
                                </v-list>
                            </v-card>
                        </v-stepper-content>
                    </v-stepper>
                  </v-flex>
                  
              </v-layout>
          </v-card>
      </v-dialog>
  </div>
</template>

<script>
export default {
    name:'pmanager',
    data(){
        return{
            password:'',
            password1:'',
            active:[],
            permissions:[
                {
                     name:"Admin",
                    desc:'Make a Admin'
                },
                {
                    name:"View",
                    desc:'Allow to view the contents'
                },
                 {
                    name:"Modify",
                    desc:'Allow to modify the contents'
                },
                 {
                    name:"Insert",
                    desc:'Allow to Insert new contents'
                },
                 {
                    name:"Delete",
                    desc:'Allow to Delete the contents'
                },
            ],
            dialog:true,
            e1:1,
            username:'',
            show1:true,
           
        }
    },
    methods:{
       
        admin(){
            //this.active[1]=true;
            if(this.permissions.length == this.active.length){
               this.active =[];
                //alert("non"+this.active.length);
            }else{
               
                  this.active =[];
                this.permissions.forEach(element => {
                    this.active.push(element.name);
                    //alert(element.name);
                });
               // alert(this.active.length);
            }
            
           // alert(this.active);
        },
       
    },
    watch:{
        active(){
            if(this.active.length !== this.permissions.length ){
              if(this.active[0] =="Admin"){
                  delete this.active[0];
              }
            }
        },
        
    },
    computed: {
        hasRole(){
            if(this.active.length>0){
                return false;
            }else{
                return true;
            }
        },
      progress () {
        return Math.min(100, this.value.length * 10)
      },
      color () {
        return ['error', 'warning', 'success'][Math.floor(this.progress / 40)]
      },
        everythingOk(){
            
            if( this.username.length>0 &&this.password === this.password1 && this.password.length >= 8 ){
                return false;
            }else{
                return true;
            }
        },
    },
}
</script>

<style>

</style>