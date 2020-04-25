<template>
    <div style="margin: 0 auto; width: 80%">
        <Panel header="CRUD 4SD">
            <DataTable :value="personas" :paginator="true" :rows="10">
                <Column field="nombre" header="Nombre"></Column>
                <Column field="apellido" header="Apellido"></Column>
                <Column field="direccion" header="Dirección"></Column>
                <Column field="telefono" header="Teléfono"></Column>
            </DataTable>
        </Panel>
    </div>
</template>

<script>
import PersonaService from '../service/PersonaService';
export default {
    name : 'CrudApp',
    data() {
        return {
            personas : null
        }
    },
    personaService : null,
    created() {
        this.personaService = new PersonaService();
    },
    mounted() {
        this.personaService.getAll().then(data => {
            this.personas = data.data;
            console.log(this.personas);
        });
    }
}
</script>

<style>

</style>