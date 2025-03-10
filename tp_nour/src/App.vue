<template>
  <header>
			<img src="images/logo-les-bonnes-pieces.png" alt="LOGO">
			<h1>Les Bonnes Pièces</h1>
    </header>
		<main>
      <div id="countainer">
			<!-- Menu de recherche -->
       <div>
        <section class="filtres">
				<h3>Filtres</h3>
        <input v-model="chercher" placeholder="Recherche ...">
        <select v-model="choisir_categories">
          <option value="">toutes les catégories</option>
          <option v-for="categorie in categories" :key="categorie">{{ categorie }}</option>

        </select>
        <select v-model="sortprix">
            <option value="asc">Prix croissant</option>
            <option value="desc">Prix décroissant</option>
        </select>
        <select v-model="showdisponible" >
          <option value="">tous</option>
          <option value="true">disponible</option>
          <option value="false">en Rupture de stock</option>
        </select>
        
			</section>
       </div>
			
      <div>
        <!-- Fiches produits -->
        <section class="fiches">
        <h3>Produits disponibles</h3>
        <ul>
          <div v-for="piece in filtres" :key="piece.id">
            <h5>{{ piece.nom }}</h5>
            <img :src="piece.image" width="150">
            <p>Prix : {{ piece.prix }} dh</p>
            <p v-if="piece.disponible">En stock</p>
            <p v-else>Rupture de stock</p>
            <button v-if="piece.disponible" v-on:click="ajouterAuPanier(piece)">Ajouter au panier</button>

</div>
        </ul>
			</section>
      </div>
      <section class="panier">
  <h3>Panier</h3>
  <ul>
    <li v-for="item in panier" :key="item.id">
      {{ item.nom }} - {{ item.prix }} dh
    </li>
  </ul>
</section>
			
    </div>
		</main>
   
</template>

<script>
import auto_data from "@/data/pieces-autos.json";

export default {
  name: 'App',
  
  data() {
    return {
      pieces: auto_data,
      choisir_categories:"",
      chercher:"",
      sortprix:"asc",
      panier: [],
      showdisponible:"",
    }
  },
  computed: {
    //recuperer les categories dans une liste categories
    categories(){
      let uniqueCategories = [];
      this.pieces.forEach(piece => {
    if (!uniqueCategories.includes(piece.categorie)) {
      uniqueCategories.push(piece.categorie);
    }
    });
    return uniqueCategories;
    },

    filtres() {
    let filtres = this.pieces;

    
    if (this.choisir_categories) {
      filtres = filtres.filter(piece => piece.categorie === this.choisir_categories);
    }

    
    if (this.showdisponible === "true") {
  filtres = filtres.filter(piece => piece.disponible === true);
} else if (this.showdisponible === "false") {
  filtres = filtres.filter(piece => piece.disponible === false);
}

    
    if (this.chercher) {
      filtres = filtres.filter(piece => piece.nom.toLowerCase().includes(this.chercher.toLowerCase()));
    }

    
    return filtres.sort((a, b) => {
      return this.sortprix === "asc" ? a.prix - b.prix : b.prix - a.prix;
    });
  }
  },
  methods: {
  ajouterAuPanier(piece) {
    this.panier.push(piece);
  }
}

  };

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
