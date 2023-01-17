<template>
  <q-page padding style="max-width: 400px">
    <q-form @submit="btn_enviar" class="q-gutter-md">
      <fieldset>
        <legend>Información personal</legend>

        <q-input
          filled
          v-model="nombre"
          label="Nombre"
          hint="Nombre(s) y apellidos *"
          lazy-rules
          :rules="[
            (val) => (val && val.length > 0) || 'Por favor escribe algo',
          ]"
          pattern="[A-Za-z]+"
          title="Solo debe contener letras"
        />
      </fieldset>

      <fieldset>
        <legend>Datos bancarios</legend>

        <q-input
          filled
          v-model="numero_tarjeta"
          label="0000 - 0000 - 0000 - 0000"
          hint="Numero de tarjeta *"
          mask="#### - #### - #### - ####"
          lazy-rules
          :rules="[
            (val) => (val !== null && val !== '') || 'Por favor escribe algo',
            (val) =>
              (val && val.length == 25) ||
              'Por favor escriba los 16 numeros de tarjeta',
          ]"
        />

        <div class="row">
          <div class="col fecha">
            <q-input
              class="q-pt-md"
              filled
              v-model="fecha_tarjeta"
              label="MM/AA"
              hint="Fecha de vencimiento *"
              mask="##/##"
              lazy-rules
              :rules="[
                (val) =>
                  (val && val.length > 0) || 'Escriba la fecha de vencimiento',
                (val) =>
                  (val && val.length == 5) || 'Por favor escriba la fecha',
              ]"
            />
          </div>

          <div class="col">
            <q-input
              class="q-pt-md cvv"
              filled
              type="tel"
              v-model="cvv"
              label="000"
              hint="Codigo CVV *"
              mask="###"
              lazy-rules
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Por favor escribe el CVV',
                (val) =>
                  (val && val.length == 3) ||
                  'Por favor escriba los 3 numeros CVV',
              ]"
            />
          </div>
        </div>
      </fieldset>

      <fieldset>
        <legend>Datos de usuario</legend>

        <q-input
          filled
          type="text"
          v-model="usuario"
          label="Usuario"
          hint="Nombre de usuario *"
          lazy-rules
          :rules="[
            (val) => (val !== null && val !== '') || 'Por favor escribe algo',
          ]"
        />

        <q-input
          class="q-pt-md"
          filled
          type="password"
          v-model="contraseña"
          label="********"
          hint="Contraseña *"
          minlength="8"
          lazy-rules
          :rules="[
            (val) => (val !== null && val !== '') || 'Por favor escribe algo',
            (val) => (val && val.length >= 8) || 'Tamaño minimo 8 caracteres',
          ]"
        />
      </fieldset>

      <div class="text-right q-pr-sm">
        <q-btn label="Enviar" type="submit" color="primary" />
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { ref } from "vue";
import { useQuasar } from "quasar";

export default {
  setup() {
    const $q = useQuasar();
    const nombre = ref(null);
    const numero_tarjeta = ref(null);
    const fecha_tarjeta = ref(null);
    const cvv = ref(null);
    const usuario = ref(null);
    const contraseña = ref(null);

    const btn_enviar = () => {
      var mayus = false;
      var minus = false;
      var num = false;
      var caract = false;

      for (var i = 0; i < contraseña.value.length; i++) {
        switch (true) {
          case contraseña.value[i].charCodeAt() >= 48 &&
            contraseña.value[i].charCodeAt() <= 57:
            num = true;
            break;

          case contraseña.value[i].charCodeAt() >= 65 &&
            contraseña.value[i].charCodeAt() <= 90:
            mayus = true;
            break;

          case contraseña.value[i].charCodeAt() >= 97 &&
            contraseña.value[i].charCodeAt() <= 122:
            minus = true;
            break;

          case (contraseña.value[i].charCodeAt() >= 33 &&
            contraseña.value[i].charCodeAt() <= 47) ||
            (contraseña.value[i].charCodeAt() >= 58 &&
              contraseña.value[i].charCodeAt() <= 64) ||
            (contraseña.value[i].charCodeAt() >= 91 &&
              contraseña.value[i].charCodeAt() <= 96) ||
            (contraseña.value[i].charCodeAt() >= 123 &&
              contraseña.value[i].charCodeAt() <= 126):
            caract = true;
            break;
        }
      }

      if (mayus && minus && num && caract) {
        $q.notify({ type: "positive", message: "Formulario correcto" });
      } else if (!mayus && minus && num && caract) {
        $q.notify({
          type: "negative",
          message: "Falta al menos una letra Mayuscula en contraseña",
        });
      } else if (mayus && !minus && num && caract) {
        $q.notify({
          type: "negative",
          message: "Falta al menos una letra minuscula en contraseña",
        });
      } else if (mayus && minus && !num && caract) {
        $q.notify({
          type: "negative",
          message: "Falta al menos un numero en contraseña",
        });
      } else if (mayus && minus && num && !caract) {
        $q.notify({
          type: "negative",
          message: "Falta al menos un caracter especial en contraseña",
        });
      } else {
        $q.notify({
          type: "negative",
          message:
            "Contraseña debe contener al menos una letra mayuscula, una minuscula, un numero y un caracter especial",
        });
      }
    };

    return {
      nombre,
      numero_tarjeta,
      fecha_tarjeta,
      cvv,
      usuario,
      contraseña,
      btn_enviar,
    };
  },
};
</script>

<style>
.cvv {
  padding-left: 15px;
}
.fecha {
  padding-bottom: 10px;
}
</style>
