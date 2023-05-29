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
            <tr v-for="nft in paginatedNfts" :key="nft.id">
                <td><img :src="nft.image" alt="Nft Image" class="w-24 h-24" /></td>
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
    <div class="pagination">
        <button @click="prevPage">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd"
                    d="M16.2071 4.29289C16.5976 4.68342 16.5976 5.31658 16.2071 5.70711L9.91421 12L16.2071 18.2929C16.5976 18.6834 16.5976 19.3166 16.2071 19.7071C15.8166 20.0976 15.1834 20.0976 14.7929 19.7071L7.79289 12.7071C7.40237 12.3166 7.40237 11.6834 7.79289 11.2929L14.7929 4.29289C15.1834 3.90237 15.8166 3.90237 16.2071 4.29289Z"
                    fill="#3F4851" />
            </svg>
        </button>
        <button @click="nextPage"><svg width="24" height="24" viewBox="0 0 24 24" fill="none"
                xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd"
                    d="M7.79289 19.7071C7.40237 19.3166 7.40237 18.6834 7.79289 18.2929L14.0858 12L7.79289 5.70711C7.40237 5.31658 7.40237 4.68342 7.79289 4.29289C8.18342 3.90237 8.81658 3.90237 9.20711 4.29289L16.2071 11.2929C16.5976 11.6834 16.5976 12.3166 16.2071 12.7071L9.20711 19.7071C8.81658 20.0976 8.18342 20.0976 7.79289 19.7071Z"
                    fill="#3F4851" />
            </svg>
        </button>
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
        paginatedNfts: function () {
            return this.nftsArray.filter((row, index) => {
                let start = (this.currentPage - 1) * this.pageSize;
                let end = this.currentPage * this.pageSize;
                if (index >= start && index < end) return true;
            });
        }
    }
}

</script>

<style></style>