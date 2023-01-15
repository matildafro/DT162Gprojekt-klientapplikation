<template>
  <!--läser in header-->
<Header />
<!--läser in AddInsert-->
<AddInsert @insertAdded="getInserts()" />
<div class="content">
  <h2>Sammanställning av kurser:</h2>
  <p>Behöver du ändra en inlagd kurs? Tryck på textfältet och sedan uppdatera-knappen.</p>
  <p id="messageGet"></p><!--meddelande om kurs ändrats eller raderats-->
<table>
  <th>Kursnamn</th>
  <th>Universitet</th>
  <th>Inriktning</th>
  <th>Poäng</th>
  <th>Uppdatera</th>
  <th>Radera</th>
  <!--läser in GetList och loopar igenom objekt inserts-->
  <GetList @deleteInsert="deleteInsert(insert._id)" @updateInsert="updateInsert(insert._id)" v-for="insert in inserts" :insert="insert" :key="insert._id"/>

</table>
<!--läser ut poängsammanställning-->
<div id="getPoints"></div>
</div>

    
</template>

<script>
import Header from './components/Header.vue'
import GetList from './components/GetList.vue'
import AddInsert from './components/AddInsert.vue'

export default {
    // objekt sparas som tom array
    data() {
        return {
            inserts: [],
        }
    },
    components: {
        GetList,
        AddInsert,
        Header 
    },

    //funktioner som använfs
    methods: {

    //GET
    async getInserts() {
               //Fetch till restwebbtjänst
            const resp = await fetch("http://localhost:3000/inserts/", {
            method: "GET",
            headers: {
                            "Content-type": "application/json"
                        }
        });  
        // När vi fått svar från API't ska data lagras
        const data = await resp.json();
        this.inserts = data;

        //HÄMTA POINTS OCH LÄS UT
        let sum = 0;
        for (let i = 0; i < data.length; i++)
        {
          sum = sum + data[i].points;
        }
        document.getElementById("getPoints").innerHTML = "<h1>Lästa poäng: " + sum + "p</h1>";
        

}, 

//Radera - DELETE
async deleteInsert(_id) {
  const resp = await fetch("http://localhost:3000/inserts/" + _id, {
            method: "DELETE",
            
            headers: {      
                            "Accept": "application/json",
                            "Content-type": "application/json"
                        }
        });  
        // När vi fått svar från webbtjänsten ska data lagras
      const data = await resp.json();
      this.getInserts();
      document.getElementById("messageGet").innerHTML = "<p>" + "Kurs raderad!" + "</p>";
},
//Uppdatera - UPDATE
async updateInsert(_id) {
  let updName = document.getElementById("name" + _id).innerHTML;
  let updUni = document.getElementById("uni" + _id).innerHTML;
  let updOrient = document.getElementById("orient" + _id).innerHTML;
  let updPoints = document.getElementById("points" + _id).innerHTML;

  let updBody = {
    name: updName,
    uni: updUni,
    orient: updOrient,
    points: updPoints
  };

  const resp = await fetch("http://localhost:3000/inserts/" + _id, {
            method: "PUT",
            headers: {      
                            "Accept": "application/json",
                            "Content-type": "application/json"
                        },
            body: JSON.stringify(updBody)
        });  
        const data = await resp.json();
        this.getInserts();
        document.getElementById("messageGet").innerHTML = "<p>" + "Kurs uppdaterad!" + "</p>";
}
},

mounted() {
  this.getInserts();

    }}
</script>

<style scoped>
.content {
  background-color: white;
    width: 70%;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 10px;
    border-radius: 3px;
    border: 2px solid rgb(105, 180, 180);
    padding: 20px;
}
th {
  font-weight: bold;
  border-bottom: 1px solid darkslategray;
}

table {
  width: 90%;
  margin-left: auto;
  margin-right: auto;
}

h1, h2, p {
  text-align: center;
}

#getPoints {
  text-align: center;
  color: rgb(105, 180, 180);
}

#messageGet {
  text-align: center;
  font-weight: bold;
}

</style>
