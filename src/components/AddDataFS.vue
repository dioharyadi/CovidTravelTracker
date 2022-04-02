<template>
    <button @click="fetchCovidCase()">test</button>
</template>
<script>
import axios from 'axios'
import firebaseApp from '../firebase.js';
import {getFirestore} from "firebase/firestore";
import {doc, setDoc} from "firebase/firestore";
const db = getFirestore(firebaseApp);


export default {
    name : 'AddDataFS',
    components : {},
    data() {
        return  {
            AU : '',
            CN : '',
            ID : '',
            JP : '',
            KR : '',
            MY : '',
            US : '',
        }
    },
    methods : {
        async  fetchCovidCase() {
            console.log("Working")
            var enddateObj = new Date()
            var startdateObj = new Date(new Date().setDate(enddateObj.getDate()-30));
            var enddate = enddateObj.toISOString();
            var startdate = startdateObj.toISOString();
            enddate = enddate.substring(0, enddate.indexOf('T')+1);
            startdate = startdate.substring(0, startdate.indexOf('T')+1);
            enddate += "00:00:00Z"
            startdate += "00:00:00Z"
            console.log(startdate, enddate)
            await axios.get('https://api.covid19api.com/total/country/' + 'AU' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.AU = response))
            this.AU.data.forEach(data => setDoc(doc(db, "Australia", data["Date"]), {
                Country: "Australia", date: data["Date"], Cases: data["Cases"]
            }))
            await axios.get('https://api.covid19api.com/total/country/' + 'CN' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.CN = response))
            this.CN.data.forEach(data => setDoc(doc(db, "China", data["Date"]), {
                Country: "China", date: data["Date"], Cases: data["Cases"]
            }))
            await axios.get('https://api.covid19api.com/total/country/' + 'ID' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.ID = response))
            this.ID.data.forEach(data => setDoc(doc(db, "Indonesia", data["Date"]), {
                Country: "Indonesia", date: data["Date"], Cases: data["Cases"]
            }))            
            await axios.get('https://api.covid19api.com/total/country/' + 'JP' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.JP = response))
            this.JP.data.forEach(data => setDoc(doc(db, "Japan", data["Date"]), {
                Country: "Japan", date: data["Date"], Cases: data["Cases"]
            }))            
            await axios.get('https://api.covid19api.com/total/country/' + 'KR' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.KR = response))
            this.KR.data.forEach(data => setDoc(doc(db, "South Korea", data["Date"]), {
                Country: "South Korea", date: data["Date"], Cases: data["Cases"]
            }))            
            await axios.get('https://api.covid19api.com/total/country/' + 'MY' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.MY = response))
            this.MY.data.forEach(data => setDoc(doc(db, "Malaysia", data["Date"]), {
                Country: "Malaysia", date: data["Date"], Cases: data["Cases"]
            }))            
            await axios.get('https://api.covid19api.com/total/country/' + 'US' + '/status/confirmed?from=' + startdate + '&to=' + enddate).then(response => (this.US = response))
            this.US.data.forEach(data => setDoc(doc(db, "USA", data["Date"]), {
                Country: "USA", date: data["Date"], Cases: data["Cases"]
            }))
        }
    },
    mounted() {
        console.log(this.AU)
    },
}
</script>
