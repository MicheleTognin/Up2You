<template>
    <table class="table-auto">
        <thead>
            <tr>
                <th>Immagine</th>
                <th>ID</th>
                <th>Azienda</th>
                <th>CO₂ (kg)</th>
                <th>Progetto</th>
                <th>Tipo</th>
                <th>Origine</th>
                <th>Data</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="nft in sortedNfts" :key="nft.id">
                <td><img :src="nft.image"/></td>
                <td>{{ nft.internal_id }}</td>
                <td>{{ nft.details.entity_name }}</td>
                <td>{{ nft.formatted_quantity_kg }}</td>
                <td>{{ nft.details.project_name }} {{ nft.standard_full }}</td>
                <td>{{ nft.details.project_type }}</td>
                <td>{{ nft.details.country_name }}</td>
                <td>{{ nft.details.transaction_datetime }}</td>
            </tr>
        </tbody>
    </table>
    <div>
        <button @click="prevPage">Previous</button>
        <button @click="nextPage">Next</button>
    </div>
</template>

<script>

export default {
    name: 'NftTable',

    data() {
        return {
            nftsArray: null,
            pageSize: 8,
            currentPage: 1
        }
    },

    created() {
        fetch('https://technical-interview-production.up.railway.app/nft')
            .then(response => response.json())
            .then(resp => {
                this.nftsArray = resp.data;
            })
    },

    methods: {
        nextPage: function () {
            if ((this.currentPage * this.pageSize) < this.nftsArray.length) this.currentPage++;
        },
        prevPage: function () {
            if (this.currentPage > 1) this.currentPage--;
        }
    },

    computed: {
        sortedNfts: function () {
            return this.nftsArray.filter((row, index) => {
                let start = (this.currentPage - 1) * this.pageSize;
                let end = this.currentPage * this.pageSize;
                if (index >= start && index < end) return true;
            });
        }
    }
}

</script>