<template>
    <section>
        <div role="alert" :class="['row', 'alert', item.estado? 'alert-success' : 'alert-secondary'] ">
            <div class="col-12 col-md-3">
                <span id="nombre">
                    {{ item.nombre}}
                </span>
            </div>
            <div class="col-12 col-md-3">
                <span>
                    {{ item.descripcion}}
                </span>
            </div>
            <div class="col-12 col-md-3">
                <span>
                    {{ (item.estado? 'Realizada' : 'Por hacer')}}
                </span>
            </div>
            <div class="col-12 col-md-3">
                <button 
                    class="btn btn-primary" 
                    @click="$emit('cambiar',index)"
                >Estado</button>
                <button 
                    class="btn btn-danger" 
                    @click="$emit('eliminar',index)"
                >Eliminar </button>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    props: {
        item : Object,
        index : Number
    },
}
</script>