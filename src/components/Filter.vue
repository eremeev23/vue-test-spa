<template>
  <div id="filter-box">
        <form @submit.prevent="" class="filter-form">
            <select class="select" name="branch" id="branch" @click="submitRegion">
                <option value="ФИЛИАЛ">ФИЛИАЛ</option>
                <option v-for='branch in list_data'
                        :key='branch.id'
                        :branch_data="branch"
                >
                {{ branch.branchName }}
                </option>
            </select>
            <select class="select" name="region" id="region" >
                <option value="РЕГИОН">РЕГИОН</option>
                <option v-for='region in regions'
                        :key='region.id'
                        :region_data="region"
                >
                {{ region.name }}
                </option>
            </select>
            <button @click="submitForm" type="submit" class="button">Показать</button>
        </form>

        <div class="list">
            <ul v-show="search">
                <li class="list-item"  v-for='vendor in vendors'
                    :key='vendor.id'
                    :vendor_data="vendor">
                    <h1 class="vendor-name">{{ vendor.name }}</h1>
                    <div class="stats">
                        <span class="active">Количество активных объектов: {{ vendor.active }}</span>
                        <span class="disabled">Количество отключенных объектов: {{ vendor.disabled }}</span>
                    </div>
                </li>
            </ul>
        </div>
  </div>
</template>

<script>
export default {
    name: 'filter-box',
    data() {
        return {
            regions: [],
            vendors: [
                {name: "ZTE", active: 0, disabled: 0},
                {name: "NOKIA", active: 0, disabled: 0},
                {name: "HUAWEI", active: 0, disabled: 0}
            ],
            search: false,
        }
    },
    props: {
        list_data: {
        type: Object,
        default: () => {}
        },
    },
    methods: {
        // API simulation
        getObjectsByRegionCode(regionCode) {
            let vendorList = ['HUAWEI', 'ZTE', 'NOKIA']
            let statusList = ['ACTIVE', 'DISABLE']
            let response = {}
            let gri = (max) => {
                return Math.floor(Math.random() * Math.floor(max))
            }
            response[regionCode] = []
            for (let i=0; i<100; i++) {
                response[regionCode].push({ vendor: vendorList[gri(3)], name: 'CELL_'+(gri(999)+1000)+'_'+gri(9), status: statusList[gri(2)]})
            }
            return response
        },
        // Filters for regions
        submitRegion() {
            const branch = document.querySelector('#branch').value;
            if (branch == 'Центральный филиал') {
                this.regions = [...this.list_data.CN.regions]
            }
            if (branch == 'Кавказский филиал') {
                this.regions = [...this.list_data.KV.regions]
            }
        },
        // Search info and stats about the region
        submitForm() {
            const region = document.querySelector('#region').value;
            if (region !== 'РЕГИОН') {
                let response = this.getObjectsByRegionCode(region.code).undefined;
                // Reset active and disabled numbers
                this.vendors[0].active = 0;        
                this.vendors[1].active = 0;        
                this.vendors[2].active = 0;   
                
                this.vendors[0].disabled = 0;
                this.vendors[1].disabled = 0;
                this.vendors[2].disabled = 0;
                // Adding active and disabled from response
                for (let i = 0; i < response.length; i++){
                    if (response[i].vendor == 'HUAWEI') {
                        if (response[i].status == 'ACTIVE') {
                            this.vendors[2].active++;
                        } else {
                            this.vendors[2].disabled++;
                        }
                    }
                    if (response[i].vendor == 'ZTE') {
                        if (response[i].status == 'ACTIVE') {
                            this.vendors[0].active++;
                        } else {
                            this.vendors[0].disabled++;
                        }
                    }
                    if (response[i].vendor == 'NOKIA') {
                        if (response[i].status == 'ACTIVE') {
                            this.vendors[1].active++;
                        } else {
                            this.vendors[1].disabled++;
                        }
                    }
                }
                // Making founded info visible
                this.search = true;
            }
        }
    }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#filter-box {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}

.filter-form{
    display: block;
    margin: 60px 60px;
    max-width: 80%;
}
.select {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    cursor: pointer;
    width: 220px;
    margin: 0 40px 10px 0;
    padding: 8px;
    background: #4482df;
    color: #fff;
    border: 1px solid #c1c1c1;
}
.button {
    cursor: pointer;
    width: 220px;
    background: #30c23c;
    color: #fff;
    text-transform: uppercase;
    text-align: center;
    padding: 8px;
    border: 1px solid #c1c1c1;
    transition: all .3s linear;
}
.button:hover {
    background: #28af34;
    border: 1px solid #4482df;
}

.list {
    width: 740px;
}
.list-item {
    width: 100%;
    min-width: 220px;
    list-style: none;
    margin: 10px 0px;
    padding: 20px;
    color: #4482df;
    border: 1px solid #c1c1c1;
}
.vendor-name {
    font-size: 22px;
    margin: 2vh 1vw;
}
.active {
    margin: 2vh 1vw;
}

@media (max-width: 924px) {
    .filter-form{
        margin: 20px 10px;
    }
    .select {
        margin: 0 5px 10px 0;
    }
}

@media (max-width: 840px) {
    .button {
        width: 120px;
    }
    .select {
        width: 160px;
    }
    .stats {
        display: flex;
        flex-direction: column;
    }
    .list {
        width: 80%;
    }
    .list-item {
        max-width: 100%;
        width: 100%;
        margin: 10px 0;
    }
    .vendor-name {
        margin: 1vh 0;
    }
    .active {
        margin: 0;
    }
}

@media (max-width: 564px) {
    .filter-form {
        margin: 0;
        display: flex;
        flex-direction: column;
        max-width: 100%;
        width: 100%;
    }
    .button, .select {
        width: 100%;
        margin: 0;
        padding: 20px 40px;
    }
    .list {
        width: 100%;
    }
    .list-item {
        max-width: 100%;
        width: 100%;
        margin: 10px 0;
    }
}
@media (max-width: 328px) {
    .vendor-name, .active {
        margin: 7px 0;
    }
}
</style>