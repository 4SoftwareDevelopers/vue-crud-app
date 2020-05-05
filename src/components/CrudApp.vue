<template>
  <div style="margin: 0 auto; width: 80%">
    <Panel header="CRUD 4SD">
      <Menubar :model="items" />
      <br />
      <DataTable :value="personas" :paginator="true" :rows="10">
        <Column field="nombre" header="Nombre"></Column>
        <Column field="apellido" header="Apellido"></Column>
        <Column field="direccion" header="Dirección"></Column>
        <Column field="telefono" header="Teléfono"></Column>
      </DataTable>
    </Panel>
    <Dialog header="Crear Persona" :visible.sync="displayModal" :modal="true">
      <span class="p-float-label">
        <InputText id="nombre" type="text" v-model="persona.nombre" style="width: 100%" />
        <label for="nombre">Nombre</label>
      </span>
      <br />
      <span class="p-float-label">
        <InputText id="apellido" type="text" v-model="persona.apellido" style="width: 100%" />
        <label for="apellido">Apellido</label>
      </span>
      <br />
      <span class="p-float-label">
        <InputText id="direccion" type="text" v-model="persona.direccion" style="width: 100%" />
        <label for="direccion">Dirección</label>
      </span>
      <br />
      <span class="p-float-label">
        <InputText id="telefono" type="text" v-model="persona.telefono" style="width: 100%" />
        <label for="telefono">Teléfono</label>
      </span>
      <template #footer>
        <Button label="Guardar" icon="pi pi-check" @click="save" />
        <Button label="Cancelar" icon="pi pi-times" @click="closeModal" class="p-button-secondary" />
      </template>
    </Dialog>
  </div>
</template>

<script>
import PersonaService from "../service/PersonaService";
export default {
  name: "CrudApp",
  data() {
    return {
      personas: null,
      persona: {
        nombre: null,
        apellido: null,
        direccion: null,
        telefono: null
      },
      items: [
        {
          label: "Nuevo",
          icon: "pi pi-fw pi-plus",
          command: () => {
            this.showSaveModal();
          }
        },
        {
          label: "Editar",
          icon: "pi pi-fw pi-pencil"
        },
        {
          label: "Eliminar",
          icon: "pi pi-fw pi-trash"
        },
        {
          label: "Refrescar",
          icon: "pi pi-fw pi-refresh",
          command: () => {
            this.getAll();
          }
        }
      ],
      displayModal: false
    };
  },
  personaService: null,
  created() {
    this.personaService = new PersonaService();
  },
  mounted() {
    this.getAll();
  },
  methods: {
    showSaveModal() {
      this.displayModal = true;
    },
    getAll() {
      this.personaService.getAll().then(data => {
        this.personas = data.data;
      });
    },
    save() {
      this.personaService.save(this.persona).then(data => {
        if (data.status === 200) {
          this.displayModal = false;
          this.persona = {
            nombre: null,
            apellido: null,
            direccion: null,
            telefono: null
          };
          this.getAll();
        }
      });
    },
    closeModal() {
      this.displayModal = false;
    }
  }
};
</script>

<style>
</style>