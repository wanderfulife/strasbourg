<template>
	<div class="container">

		<div id="enqueteur" v-if="level === 0">
			<h2>Prénom enqueteur :</h2>
			<input class="form-control" type="text" v-model="ENQUETEUR" />
			<button v-if="ENQUETEUR" @click="next" class="btn-next">Suivant</button>
		</div>

		<div v-if="level === 1">
			<button @click="startSurvey" class="btn-next">COMMENCER QUESTIONNAIRE</button>
		</div>

		<div id="q1" v-if="level === 2">
			<h1> Habitez-vous dans la rue ? </h1>
			<select v-model="Q1" class="form-control">
				<option v-for="option in q1" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q1" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q2" v-if="level === 3 && Q1 === 2">
			<h1>Vous vous déplacez aujourd'hui pour …</h1>
			<select v-model="Q2" class="form-control">
				<option v-for="option in q2" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<input v-if="Q2 === 5" class="form-control" type="text" v-model="Q2_DETAIL" placeholder="Precisions">
			<button v-if="Q2" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q3" v-if="level === 4 && Q1 === 2">
			<h1> Vous passez sur cet axe ? </h1>
			<select v-model="Q3" class="form-control">
				<option v-for="option in q3" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q3" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q4" v-if="level === 5 && Q1 === 2 || level === 3 && Q1 === 1">
			<h1> Quel est votre moyen de transport principal ? </h1>
			<select v-model="Q4" class="form-control">
				<option v-for="option in q4" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q4" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q5" v-if="level === 6 && Q1 === 2 || level === 4 && Q1 === 1">
			<h1> Savez-vous que vous êtes dans une zone de rencontre ?</h1>
			<select v-model="Q5" class="form-control">
				<option v-for="option in q5" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q5" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q6" v-if="(level === 7 && Q1 === 2 || level === 5 && Q1 === 1) && Q5 === 1">
			<h1> Comment ?</h1>
			<br>
			<label for="1">Par les panneaux de signalisation</label>
			<input type="checkbox" id="1" value="1" v-model="Q6">
			<br>
			<label for="2">Par l’aménagement (absence de trottoir)</label>
			<input type="checkbox" id="2" value="2" v-model="Q6">
			<br>
			<label for="3">Par le marquage au sol</label>
			<input type="checkbox" id="3" value="3" v-model="Q6">
			<br>
			<label for="4">Par la limitation de vitesse</label>
			<input type="checkbox" id="4" value="4" v-model="Q6">
			<br>
			<label for="5">Autre (précisez)</label>
			<input type="checkbox" id="5" value="5" v-model="Q6">
			<br>
			<input v-if="Q6.includes('5')" class="form-control" type="text" v-model="Q6_DETAIL" placeholder="Precisez">
			<button v-if="Q6" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q7" v-if="(level === 7 && Q1 === 2 || level === 5 && Q1 === 1) && Q5 === 2">
			<h1> Connaissez-vous le principe
				d’une zone de rencontre ? </h1>
			<select v-model="Q7" class="form-control">
				<option v-for="option in q7" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q7" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q8" v-if="level === 8 && Q1 === 2 || level === 6 && Q1 === 1">
			<h1> Selon vous, qui est prioritaire dans une zone de
				rencontre ? </h1>
			<select v-model="Q8" class="form-control">
				<option v-for="option in q8" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q8" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>


		<div id="q9" v-if="level === 9 && Q1 === 2 || level === 7 && Q1 === 1">
			<h1>Quelle est la vitesse maximale autorisée dans une zone de
				rencontre ?</h1>
			<select v-model="Q9" class="form-control">
				<option v-for="option in q9" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q9" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q10" v-if="level === 10 && Q1 === 2 || level === 8 && Q1 === 1">
			<h1> Selon vous, la sécurité des usagers est-elle renforcée
				avec la mise en place de zone de rencontre ?</h1>
			<select v-model="Q10" class="form-control">
				<option v-for="option in q10" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q10" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>


		<div id="q11" v-if="level === 11 && Q1 === 2 || level === 9 && Q1 === 1">
			<h1>Pourquoi ?</h1>
			<br>
			<label for="1">Apaisement des vitesses de véhicules motorisés</label>
			<input type="checkbox" id="1" value="1" v-model="Q11">
			<br>
			<label for="2">Baisse du trafic</label>
			<input type="checkbox" id="2" value="2" v-model="Q11">
			<br>
			<label for="3">Meilleur partage de l’espace public</label>
			<input type="checkbox" id="3" value="3" v-model="Q11">
			<br>
			<label for="4">Moins de conflits d’usage</label>
			<input type="checkbox" id="4" value="4" v-model="Q11">
			<br>
			<label for="5">Trop grande proximité des piétons et des cyclistes
				avec le trafic automobile</label>
			<input type="checkbox" id="5" value="5" v-model="Q11">
			<br>
			<label for="6">Suppression des trottoirs</label>
			<input type="checkbox" id="6" value="6" v-model="Q11">
			<br>
			<label for="7">Non-respect des vitesses de véhicules motorisés</label>
			<input type="checkbox" id="7" value="7" v-model="Q11">
			<br>
			<label for="8">Stationnement sauvage (livraisons et autre, …)</label>
			<input type="checkbox" id="8" value="8" v-model="Q11">
			<br>
			<label for="9">Absence de sentiment de priorité pour circuler en tant
				que piétons et cyclistes</label>
			<input type="checkbox" id="9" value="9" v-model="Q11">
			<br>
			<label for="10">Autre (précisez)</label>
			<input type="checkbox" id="10" value="10" v-model="Q11">
			<br>

			<input v-if="Q11.includes('10')" class="form-control" type="text" v-model="Q11_DETAIL"
				placeholder="Precisez">
			<button v-if="Q11" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q12" v-if="level === 12 && Q1 === 2 || level === 10 && Q1 === 1">
			<h1> Selon vous, le confort (qualité de l'aménagement) des
				piétons et des cyclistes est-il renforcé ?</h1>
			<select v-model="Q12" class="form-control">
				<option v-for="option in q12" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q12" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>


		<div id="q13" v-if="level === 13 && Q1 === 2 || level === 11 && Q1 === 1">
			<h1>Pourquoi ?</h1>
			<br>
			<label for="1">Diminution des nuisances sonores</label>
			<input type="checkbox" id="1" value="1" v-model="Q13">
			<br>
			<label for="2">Espace plus convivial</label>
			<input type="checkbox" id="2" value="2" v-model="Q13">
			<br>
			<label for="3">Cheminements et traversées facilités</label>
			<input type="checkbox" id="3" value="3" v-model="Q13">
			<br>
			<label for="4">Absence de sentiment de priorité pour circuler en tant
				que piétons et cyclistes</label>
			<input type="checkbox" id="4" value="4" v-model="Q13">
			<br>
			<label for="5">Autre (précisez)</label>
			<input type="checkbox" id="5" value="5" v-model="Q13">
			<br>
			<input v-if="Q13.includes('5')" class="form-control" type="text" v-model="Q13_DETAIL"
				placeholder="Precisez">
			<button v-if="Q13" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>


		<div id="q14" v-if="level === 14 && Q1 === 2 || level === 12 && Q1 === 1">
			<h1> Êtes-vous commerçant dans une zone de rencontre ? </h1>
			<select v-model="Q14" class="form-control">
				<option v-for="option in q14" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q14" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div id="q15" v-if="(level === 15 && Q1 === 2 || level === 13 && Q1 === 1) && Q14 === 1">
			<h1> Être dans une zone de rencontre est-il avantageux pour votre activité ?</h1>
			<select v-model="Q15" class="form-control">
				<option v-for="option in q15" :key="option.id" :value="option.output">
					{{ option.text }}
				</option>
			</select>
			<button v-if="Q15" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>


		<div id="q16" v-if="((level === 16 && Q1 === 2 || level === 14 && Q1 === 1) && Q14 === 1) ||
	((level === 15 && Q1 === 2 || level === 13 && Q1 === 1) && Q14 === 2)">
			<h1>Pourquoi ?</h1>
			<br>
			<label for="1">Plus de passage</label>
			<input type="checkbox" id="1" value="1" v-model="Q16">
			<br>
			<label for="2">Espace plus convivial</label>
			<input type="checkbox" id="2" value="2" v-model="Q16">
			<br>
			<label for="3">Moins de passage</label>
			<input type="checkbox" id="3" value="3" v-model="Q16">
			<br>
			<label for="4">Aucun changement</label>
			<input type="checkbox" id="4" value="4" v-model="Q16">
			<br>
			<label for="5">Difficultés supplémentaires concernant les livraisons et l’accessibilité</label>
			<input type="checkbox" id="5" value="5" v-model="Q16">
			<br>
			<label for="6">Autre (précisez)</label>
			<input type="checkbox" id="6" value="6" v-model="Q16">
			<br>
			<input v-if="Q16.includes('6')" class="form-control" type="text" v-model="Q16_DETAIL"
				placeholder="Precisez">
			<button v-if="Q16" @click="next" class="btn-next">Suivant</button>
			<button @click="back" class="btn-return">retour</button>
		</div>

		<div>
			<button @click="submitSurvey" class="btn-next">FINIR QUESTIONNAIRE</button>
			<button @click="back" class="btn-return">retour</button>
		</div>
		<img class="logo" src="../assets/Alycelogo.webp" alt="Logo Alyce">

		<button class="btn-fin" @click="downloadData">download DATA</button>

	</div>
	<div>
		<span style="margin-left: 10px;">Nombre de questionnaires : {{ docCount }}</span>
	</div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { q1, q2, q3, q4, q5, q6, q7, q8, q9, q10, q11, q12, q13, q14, q15, q16, age, sexe } from "./reponses";
import GareSelector from "./GareSelector.vue";
import CommuneSelector from './CommuneSelector.vue';
import { db } from "../firebaseConfig";
import { collection, getDocs, addDoc } from "firebase/firestore";
import * as XLSX from "xlsx";

const docCount = ref(0);
const surveyCollectionRef = collection(db, "Strasbourg");
const level = ref(0);
const startDate = ref('');
const ENQUETEUR = ref('');
const Q1 = ref('');
const Q2 = ref('');
const Q2_DETAIL = ref('');
const Q3 = ref('');
const Q4 = ref('');
const Q5 = ref('');
const Q6 = ref([]);
const Q6_DETAIL = ref('');
const Q7 = ref('');
const Q8 = ref('');
const Q9 = ref('');
const Q10 = ref('');
const Q11 = ref([]);
const Q11_DETAIL = ref('');
const Q12 = ref('');
const Q13 = ref([]);
const Q13_DETAIL = ref('');
const Q14 = ref('');
const Q15 = ref('');
const Q16 = ref([]);
const Q16_DETAIL = ref('');




const startSurvey = () => {
	startDate.value = new Date().toLocaleTimeString("fr-FR").slice(0, 8);
	level.value++;
}


const next = () => {
	level.value++;
	console.log(level.value)
}

const back = () => {
	level.value--;
}


const getDocCount = async () => {
	try {
		const querySnapshot = await getDocs(surveyCollectionRef);
		docCount.value = querySnapshot.size;
	} catch (error) {
		console.error("Error getting document count:", error);
	}
};

const submitSurvey = async () => {
	await addDoc(surveyCollectionRef, {
		HEURE_DEBUT: startDate.value,
		DATE: new Date().toLocaleDateString("fr-FR").replace(/\//g, "-"),
		JOUR: new Date().toLocaleDateString("fr-FR", { weekday: 'long' }),
		ENQUETEUR: ENQUETEUR.value,
		HEURE_FIN: new Date().toLocaleTimeString("fr-FR").slice(0, 8),
		Q1: Q1.value,
		Q2: Q2.value,
		Q2_DETAIL: Q2_DETAIL.value,
		Q3: Q3.value,
		Q4: Q4.value,
		Q5: Q5.value,
		Q6: Q6.value,
		Q6_DETAIL: Q6_DETAIL.value,
		Q7: Q7.value,
		Q8: Q8.value,
		Q9: Q9.value,
		Q10: Q10.value,
		Q11: Q11.value,
		Q11_DETAIL: Q11_DETAIL.value,
		Q12: Q12.value,
		Q13: Q13.value,
		Q13_DETAIL: Q13_DETAIL.value,
		Q14: Q14.value,
		Q15: Q15.value,
		Q16: Q16.value,
		Q16_DETAIL: Q16_DETAIL.value,

	});
	level.value = 1;
	startDate.value = "";
	getDocCount();
	Q1.value = "";
	Q2.value = "";
	Q2_DETAIL.value = "";
	Q3.value = "";
	Q4.value = "";
	Q5.value = "";
	Q6.value = [];
	Q6_DETAIL.value = "";
	Q7.value = "";
	Q8.value = "";
	Q9.value = "";
	Q10.value = "";
	Q11.value = [];
	Q11_DETAIL.value = "";
	Q12.value = "";
	Q13.value = [];
	Q13_DETAIL.value = "";
	Q14.value = "";
	Q15.value = "";
	Q16.value = [];
	Q16_DETAIL.value = "";
};

const downloadData = async () => {
	try {
		const querySnapshot = await getDocs(surveyCollectionRef);
		let data = [];
		let maxWidths = {}; // Object to keep track of maximum width for each column

		// Define your headers // MODIFICATION SUR L'EXCEL
		const headers = {
			ID_questionnaire: "ID_questionnaire",
			ENQUETEUR: "ENQUETEUR",
			DATE: "DATE",
			JOUR: "JOUR",
			HEURE_DEBUT: "HEURE_DEBUT",
			HEURE_FIN: "HEURE_FIN",
			Q1: "Q1",
			Q2: "Q2",
			Q2_DETAIL: "Q2_DETAIL",
			Q3: "Q3",
			Q4: "Q4",
			Q5: "Q5",
			Q6: "Q6",
			Q6_DETAIL: "Q6_DETAIL",
			Q7: "Q7",
			Q8: "Q8",
			Q9: "Q9",
			Q10: "Q10",
			Q11: "Q11",
			Q11_DETAIL: "Q11_DETAIL",
			Q12: "Q12",
			Q13: "Q13",
			Q13_DETAIL: "Q13_DETAIL",
			Q14: "Q14",
			Q15: "Q15",
			Q16: "Q16",
			Q16_DETAIL: "Q16_DETAIL",
		};

		// Initialize maxWidths with header lengths
		Object.keys(headers).forEach((key) => {
			maxWidths[key] = headers[key].length;
		});

		querySnapshot.forEach((doc) => {
			let docData = doc.data();
			let mappedData = {
				ID_questionnaire: doc.id,
				ENQUETEUR: docData.ENQUETEUR || "",
				DATE: docData.DATE || "",
				JOUR: docData.JOUR || "",
				HEURE_DEBUT: docData.HEURE_DEBUT || "",
				HEURE_FIN: docData.HEURE_FIN || "",
				Q1: docData.Q1 || "",
				Q2: docData.Q2 || "",
				Q2_DETAIL: docData.Q2_DETAIL || "",
				Q3: docData.Q3 || "",
				Q4: docData.Q4 || "",
				Q5: docData.Q5 || "",
				Q6: docData.Q6 || "",
				Q6_DETAIL: docData.Q6_DETAIL || "",
				Q7: docData.Q7 || "",
				Q8: docData.Q8 || "",
				Q9: docData.Q9 || "",
				Q10: docData.Q10 || "",
				Q11: docData.Q11 || "",
				Q11_DETAIL: docData.Q11_DETAIL || "",
				Q12: docData.Q12 || "",
				Q13: docData.Q13 || "",
				Q13_DETAIL: docData.Q13_DETAIL || "",
				Q14: docData.Q14 || "",
				Q15: docData.Q15 || "",
				Q16: docData.Q16 || "",
				Q16_DETAIL: docData.Q16_DETAIL || "",
			};

			if (docData.Q6) {
				let Q6String = "";
				for (const key in docData.Q6) {
					const value = docData.Q6[key];
					// You can customize the separator here (e.g., comma, semicolon)
					Q6String += `${value}, `;
				}
				// Remove the trailing comma and space from the string
				mappedData.Q6 = Q6String.slice(0, -2);
			}


			if (docData.Q11) {
				let Q11String = "";
				for (const key in docData.Q11) {
					const value = docData.Q11[key];
					// You can customize the separator here (e.g., comma, semicolon)
					Q11String += `${value}, `;
				}
				// Remove the trailing comma and space from the string
				mappedData.Q11 = Q11String.slice(0, -2);
			}

			if (docData.Q13) {
				let Q13String = "";
				for (const key in docData.Q13) {
					const value = docData.Q13[key];
					// You can customize the separator here (e.g., comma, semicolon)
					Q13String += `${value}, `;
				}
				// Remove the trailing comma and space from the string
				mappedData.Q13 = Q13String.slice(0, -2);
			}

			if (docData.Q16) {
				let Q16String = "";
				for (const key in docData.Q16) {
					const value = docData.Q16[key];
					// You can customize the separator here (e.g., comma, semicolon)
					Q16String += `${value}, `;
				}
				// Remove the trailing comma and space from the string
				mappedData.Q16 = Q16String.slice(0, -2);
			}

			data.push(mappedData);
			// Update maxWidths for each key in mappedData
			Object.keys(mappedData).forEach((key) => {
				const valueLength = mappedData[key].toString().length;
				maxWidths[key] = Math.max(maxWidths[key], valueLength);
			});
		});
		// Convert data to a worksheet
		const worksheet = XLSX.utils.json_to_sheet(data, {
			header: Object.keys(headers),
			skipHeader: false,
		});
		// Set the widths for each column
		worksheet["!cols"] = Object.keys(maxWidths).map((key) => ({
			wch: maxWidths[key] + 2 // +2 for a little extra padding
		}));
		const workbook = XLSX.utils.book_new();
		XLSX.utils.book_append_sheet(workbook, worksheet, "Data"); ``
		// Export the workbook to a .xlsx file
		XLSX.writeFile(workbook, "Strasbourg.xlsx");
	} catch (error) {
		console.error("Error downloading data: ", error);
	}
};

</script>

<style>
body {
	background-color: #2a3b63;
}

.logo {
	padding: 10%;
	height: 3em;
}

h1 {
	text-align: center;
	color: #4caf50;
	font-size: 18px;
}

h2 {
	color: white;
	font-size: 16px;
}

.english {
	color: cyan;
}

.container {
	background-color: #2a3b63;
	color: white;
	padding: 5% 0;
	width: 75%;
	margin: auto;
}

.btn-next {
	width: 100%;
	background-color: green;
	color: white;
	padding: 20px 20px;
	margin-top: 20%;
	border: none;
	border-radius: 5px;
	cursor: pointer;
}


.btn-fin {
	width: 100%;
	background-color: #4c4faf;
	color: white;
	padding: 20px 20px;
	margin-top: 5%;
	border: none;
	border-radius: 5px;
	cursor: pointer;
}

.btn-return {
	width: 100%;
	background-color: #898989;
	color: white;
	padding: 20px 20px;
	margin-top: 5%;
	border: none;
	border-radius: 5px;
	cursor: pointer;
}

.btn-return:hover {
	background-color: #839684;
}

.commune-dropdown {
	/* Style your dropdown list here */
	list-style-type: none;
	padding: 0;
	margin: 0;
	border: 1px solid #ccc;
	border-radius: 4px;
	max-height: 200px;
	overflow-y: auto;
}

.form-control {
	width: 100%;
	border-radius: 5px;
	border: 1px solid white;
	background-color: #333;
	color: white;
	text-transform: uppercase;
	font-weight: bolder;
}

input.form-control {
	width: 93%;
}

.commune-dropdown li {
	padding: 5px 10px;
	cursor: pointer;
}

*:focus {
	outline: none;
}

.commune-dropdown li:hover {
	background-color: #f0f0f0;
}

input,
select,
button {
	font-size: 16px;
	padding: 10px;
}
</style>
