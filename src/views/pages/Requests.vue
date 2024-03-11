<template>
    <div class="card flex flex-column">
        <div class="flex">
            <div class="col-2" style="padding: 0px;">
                <Dropdown v-model="selectedMethod" :options="methods" optionLabel="name" placeholder="Select a City"
                    class="input-dog" />
            </div>
            <div class="col-8" style="padding: 0px;">
                <InputText type="text" v-model="url" class="input-dog" />
            </div>
            <div class="col-2" style="padding: 0px;">
                <Button label="Submit" class="input-dog" @click="sendRequest" />
            </div>
        </div>
        <div class="divider-vertical"></div>
        <FloatLabel>
            <Textarea v-model="inputBody" rows="15" style="width: 100%;" />
            <label>Body</label>
        </FloatLabel>
        <div class="divider-vertical"></div>
        <JsonViewer :value="jsonData" :expand-depth=5 copyable class="card">
        </JsonViewer>
    </div>
</template>

<script>
import Backend from '../../service/Backend';

export default {
    data() {
        return {
            jsonData: null,
            inputBody: "",
            url: "http://localhost:7963/test/get",
            selectedMethod: { name: "GET" },
            methods: [
                { name: "GET", },
                { name: "POST", },
                { name: "PATCH", },
                { name: "DELETE", },
                { name: "CAIO", }
            ]
        };
    },
    methods: {
        sendRequest() {
            console.log("method: " + this.selectedMethod.name)
            console.log("url: " + this.url)
            console.log("inputBody: " + this.inputBody)
            let inputParsed = null
            if (this.inputBody != "" && this.inputBody != null && this.inputBody != undefined) {
                inputParsed = JSON.parse(this.inputBody)
            }
            Backend.request("post", "api/execute", { method: this.selectedMethod.name, url: this.url, payload: inputParsed }).then((resp) => {
                console.log(resp)
                this.jsonData = resp.data.response
            })
        }
    },
};
</script>

<style>
.input-dog {
    width: 100%;
}

.divider-vertical {
    height: 20px;
}
</style>
