<template>
    <div v-if="showModalComp">
        <modal title="Settings" v-model="showModalComp">
            <div>
                Sidebar Width:
                <input type="text" :value="sidebarWidth" disabled>
            </div>
            <div style="padding-top: 1rem"></div>
            <div>
                Request Panel Ratio:
                <input type="text" :value="requestPanelRatio" disabled>
            </div>
            <div style="padding-top: 1rem"></div>
            <div>
                Response Panel Ratio:
                <input type="text" :value="responsePanelRatio" disabled>
            </div>
            <div style="padding-top: 1rem"></div>
            <div>
                <button @click="resetSettings('widths')">Reset Widths</button>
                <button @click="resetSettings()" style="margin-left: 1rem">Reset All</button>
            </div>
        </modal>
    </div>
</template>

<script>
import Modal from '@/components/Modal.vue'
import constants from '../../constants'

export default {
    props: {
        showModal: Boolean
    },
    components: {
        Modal
    },
    data() {
        return {
            sidebarWidth: null,
            requestPanelRatio: null,
            responsePanelRatio: null
        }
    },
    computed: {
        showModalComp: {
            get() {
                return this.showModal
            },
            set(value) {
                this.$emit('update:showModal', value)
            }
        }
    },
    watch: {
        showModal() {
            this.fetchSavedSettings()
        }
    },
    methods: {
        resetWidths() {
            localStorage.removeItem(constants.LOCAL_STORAGE_KEY.SIDEBAR_WIDTH)
            localStorage.removeItem(constants.LOCAL_STORAGE_KEY.REQUEST_PANEL_RATIO)
            localStorage.removeItem(constants.LOCAL_STORAGE_KEY.RESPONSE_PANEL_RATIO)
        },
        resetLayout() {
            localStorage.removeItem(constants.LOCAL_STORAGE_KEY.REQUEST_RESPONSE_LAYOUT)
        },
        resetSettings(target=null) {
            if(target) {
                if(target === 'widths') {
                    this.resetWidths()
                }

                document.location.reload()
            }

            this.resetWidths()
            this.resetLayout()
            document.location.reload()
        },
        fetchSavedSettings() {
            const savedSidebarWidth = localStorage.getItem(constants.LOCAL_STORAGE_KEY.SIDEBAR_WIDTH)
            const savedRequestPanelRatio = localStorage.getItem(constants.LOCAL_STORAGE_KEY.REQUEST_PANEL_RATIO)
            const savedResponsePanelRatio = localStorage.getItem(constants.LOCAL_STORAGE_KEY.RESPONSE_PANEL_RATIO)

            if(savedSidebarWidth) {
                this.sidebarWidth = savedSidebarWidth
            }

            if(savedRequestPanelRatio) {
                this.requestPanelRatio = savedRequestPanelRatio
            }

            if(savedResponsePanelRatio) {
                this.responsePanelRatio = savedResponsePanelRatio
            }
        }
    }
}
</script>
