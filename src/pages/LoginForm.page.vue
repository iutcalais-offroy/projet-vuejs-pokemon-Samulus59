<script setup>

import { ref } from "vue";
import axios from "axios";

const email_inscription = ref("");
const mdp_inscription = ref("");
const verif_mdp = ref("");
const email_connexion = ref("");
const mdp_connexion = ref("");


const inscription = async () => 
{
  if (mdp_inscription.value !== verif_mdp.value) 
  {
    console.error("Les mots de passe ne correspondent pas.");
    return;
  }

  try 
  {
    const response = await axios.post("https://pokemon-api-seyrinian-production.up.railway.app/users", 
    {
      email: email_inscription.value,
      password: mdp_inscription.value,
    });

    console.log("Inscription réussie", response.data);
    window.location.reload()
    } 
  catch (error) 
  {
    console.error("Erreur lors de l'inscription:", error);
  }
};

const connexion = async () => 
{
  try 
  {
    const response = await axios.post("https://pokemon-api-seyrinian-production.up.railway.app/users/login", 
    {
      email: email_connexion.value,
      password: mdp_connexion.value,
    });

    console.log("Connexion réussie", response.data);
    localStorage.token = response.token;
    window.location.href='/deck-builder';
  } 
  catch (error) 
  {
    console.error("Erreur lors de la connexion:", error);
  }
};
</script>

<template>
  <n-card>
    <n-tabs
      class="card-tabs"
      default-value="signin"
      size="large"
      animated
      pane-wrapper-style="margin: 0 -4px"
      pane-style="padding-left: 4px; padding-right: 4px; box-sizing: border-box;"
    >
      <n-tab-pane name="signin" tab="Connexion">
        <n-form>
          <n-form-item-row label="Email">
            <n-input v-model:value="email_connexion" placeholder="Entrez votre email" />
          </n-form-item-row>
          <n-form-item-row label="Mot de passe">
            <n-input v-model:value="mdp_connexion" placeholder="Entrez votre mot de passe" />
          </n-form-item-row>
        </n-form>
        <n-button type="primary" block secondary strong @click="connexion">
          Se connecter
        </n-button>
      </n-tab-pane>
      <n-tab-pane name="signup" tab="Inscription">
        <n-form>
          <n-form-item-row label="Email">
            <n-input v-model:value="email_inscription" placeholder="Entrez votre email" />
          </n-form-item-row>
          <n-form-item-row label="Mot de passe">
            <n-input type="password" v-model:value="mdp_inscription" placeholder="Entrez votre mot de passe" />
          </n-form-item-row>
          <n-form-item-row label="Confirmer mot de passe">
            <n-input type="password" v-model:value="verif_mdp" placeholder="Confirmez votre mot de passe" />
          </n-form-item-row>
        </n-form>
        <n-button type="primary" block secondary strong @click="inscription">
          Sign up
        </n-button>
      </n-tab-pane>
    </n-tabs>
  </n-card>
</template>

<style scoped>
.card-tabs .n-tabs-nav--bar-type {
  padding-left: 4px;
}
</style>