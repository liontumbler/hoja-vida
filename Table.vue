<template>
    <div class="w-100">
        <div class="">
            <div class="d-none d-sm-block">
                <div class="row bg-white border rounded my-2 overflow-hidden">
                    <div class="col-12 bg-primary text-white">
                        <div class="row fw-bold border-bottom">
                            <div :class="campo.width" v-for="(campo, indexCampos) in campos" :key="indexCampos" class="py-2 border-start border-end">
                                <div v-if="campo.type == 'text' || campo.type == 'component'" class="text-truncate" :title="campo.label">
                                    {{ campo.label }}
                                </div>
                                <div v-else-if="campo.type == 'description'" class="text-truncate" :title="campo.label"
                                    data-bs-toggle="tooltip" data-bs-placement="top" :data-bs-title="campo.label">
                                    {{ campo.label }}
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-12">
                        <div v-for="(item, indexData) in data" :key="indexData" class="row border-bottom">
                            <div :class="campo.width" v-for="(campo, indexCampos) in campos" :key="indexCampos" class="py-2 border-start border-end">
                                <div v-if="campo.type == 'text'" class="text-truncate" :title="getNestedProperty(item, campo.identificador)">
                                    {{ getNestedProperty(item, campo.identificador) }}
                                </div>
                                <div v-else-if="campo.type == 'description'" class="text-truncate" :title="getNestedProperty(item, campo.identificador)" 
                                    data-bs-toggle="tooltip" data-bs-placement="top" :data-bs-title="getNestedProperty(item, campo.identificador)">
                                    {{ getNestedProperty(item, campo.identificador) }}
                                </div>
                                <div v-else-if="campo.type == 'component'" class="text-truncate">
                                    <component :is="campo.component" v-bind="campo.props" :item="item"/>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <!--moviles-->
            <div class="d-block d-sm-none">
                <div class="row">
                    <div v-for="(item, indexData) in data" :key="indexData" class="col-xxl-3 col-md-4 col-sm-12 bg-white border rounded my-2 py-2 overflow-hidden">
                        <div v-for="(campo, indexCampos) in campos" :key="indexCampos" class="row ">
                            <div class="fw-bold col-6">
                                {{ campo.label }}
                            </div>
                            <div class="col-6" v-if="campo.type != 'component'">
                                {{ getNestedProperty(item, campo.identificador) }}
                            </div>
                            <div class="col-6" v-else-if="campo.type == 'component'">
                                <component :is="campo.component" v-bind="campo.props"/>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
    props: {
        campos: Array<{
            identificador?: string | null, 
            label: string, 
            type: 'text' | 'description' | 'component' | 'action-RUD',
            width: 'col-1' | 'col-2' | 'col-3' | 'col-4' | 'col-5' | 'col-6' | 'col-7' | 'col-8' | 'col-9' | 'col-10' | 'col-11' | 'col-12', 
            head?: 'center' | null,
            body?: 'center' | null,
            component?: any,
            props?: object

        }>,
        data: Array<any>,
    },
    methods: {
        getNestedProperty(obj: any, path: string): any {
            return path.split('.').reduce((acc, part) => acc && acc[part], obj);
        },
    },
})
</script>