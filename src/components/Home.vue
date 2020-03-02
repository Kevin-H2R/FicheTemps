<template>
    <v-container class="home">
        <v-row justify="center" class="text-center home__title">
            <h1>Génération email hedbomadaire</h1>
        </v-row>
        <!-- Monday -->
        <v-row justify="center" align="center" class="flex-sm-wrap">
            <v-col cols="6" md="3" class="text-center">{{ monday }}</v-col>
            <v-col cols="6" md="3" class="text-center">
                <v-btn-toggle v-model="mondayWorkedDay" borderless shaped>
                    <v-btn value="1">1</v-btn>
                    <v-btn value="1/2">1/2</v-btn>
                    <v-btn value="0">0</v-btn>
                </v-btn-toggle>
            </v-col>
            <v-col cols="12" md="3" class="text-center">
                <v-text-field outlined v-model="mondayComment" label="Commentaire" color="success"></v-text-field>
            </v-col>
        </v-row>
        
        <!-- Tuesday -->
        <v-row justify="center" align="center" class="flex-sm-wrap">
            <v-col cols="6" md="3" class="text-center">{{ tuesday }}</v-col>
            <v-col cols="6" md="3" class="text-center">
                <v-btn-toggle v-model="tuesdayWorkedDay" borderless shaped>
                    <v-btn value="1">1</v-btn>
                    <v-btn value="1/2">1/2</v-btn>
                    <v-btn value="0">0</v-btn>
                </v-btn-toggle>
            </v-col>
            <v-col cols="12" md="3" class="text-center">
                <v-text-field outlined v-model="tuesdayComment" label="Commentaire" color="success"></v-text-field>
            </v-col>
        </v-row>
        
        <!-- Wednesday -->
        <v-row justify="center" align="center" class="flex-sm-wrap">
            <v-col cols="6" md="3" class="text-center">{{ wednesday }}</v-col>
            <v-col cols="6" md="3" class="text-center">
                <v-btn-toggle v-model="wednesdayWorkedDay" borderless shaped>
                    <v-btn value="1">1</v-btn>
                    <v-btn value="1/2">1/2</v-btn>
                    <v-btn value="0">0</v-btn>
                </v-btn-toggle>
            </v-col>
            <v-col cols="12" md="3" class="text-center">
                <v-text-field outlined v-model="wednesdayComment" label="Commentaire" color="success"></v-text-field>
            </v-col>
        </v-row>
        
        <!-- Thursday -->
        <v-row justify="center" align="center" class="flex-sm-wrap">
            <v-col cols="6" md="3" class="text-center">{{ thursday }}</v-col>
            <v-col cols="6" md="3" class="text-center">
                <v-btn-toggle v-model="thursdayWorkedDay" borderless shaped>
                    <v-btn value="1">1</v-btn>
                    <v-btn value="1/2">1/2</v-btn>
                    <v-btn value="0">0</v-btn>
                </v-btn-toggle>
            </v-col>
            <v-col cols="12" md="3" class="text-center">
                <v-text-field outlined v-model="thursdayComment" label="Commentaire" color="success"></v-text-field>
            </v-col>
        </v-row>
        
        <!-- Friday -->
        <v-row justify="center" align="center" class="flex-sm-wrap">
            <v-col cols="6" md="3" class="text-center">{{ friday }}</v-col>
            <v-col cols="6" md="3" class="text-center">
                <v-btn-toggle v-model="fridayWorkedDay" borderless shaped>
                    <v-btn value="1">1</v-btn>
                    <v-btn value="1/2">1/2</v-btn>
                    <v-btn value="0">0</v-btn>
                </v-btn-toggle>
            </v-col>
            <v-col cols="12" md="3" class="text-center">
                <v-text-field outlined v-model="fridayComment" label="Commentaire" color="success"></v-text-field>
            </v-col>
        </v-row>
        <v-row justify="center">
            <v-col cols="12" md="6" lg="4" xl="3">
                <v-btn :href="formatMailHref" color="success" width="100%">
                    Envoyer le mail
                </v-btn>
            </v-col>
        </v-row>
        <v-row justify="center" align="center">
            <v-col cols="12" md="6" lg="4" xl="3">
                <v-dialog v-model="dialog" width="500">
                    <template v-slot:activator="{ on }">
                        <v-btn width="100%" v-on="on">Prévisualiser</v-btn>
                    </template>
                    <my-mail :object="getObject" :body="getBody"/>
                </v-dialog>
            </v-col>
        </v-row>
    </v-container>
</template>
<script>
import MyMail from './MyMail'
import config from '@/misc/mail.config.js'
export default {
    name: 'Home',
    components: {MyMail},
    created: function () {
        let lastWeekMonday = new Date()
        lastWeekMonday.setDate(lastWeekMonday.getDate() - 7 - (lastWeekMonday.getDay() + 6) % 7)
    },
    computed: {
        getObject: function () {
            return `Kevin Herrbach - CRA semaine ${this.week} ${this.year}`
        },
        getBody: function () {
            const assocWorkedDay = {'1': 'Présent', '1/2': 'Demi-Journée', '0': 'Absent'}
            let result = `Bonjour Bertrand,<br>Ci-dessous tu trouveras mon compte rendu d'activité pour la semaine ${this.week} de ${this.year}:`
            result +=  `<br><br>&nbsp&nbsp&nbsp&nbsp- ${this.monday}: ${assocWorkedDay[this.mondayWorkedDay]}`
            if (this.mondayComment !== null) {
                result += ` (${this.mondayComment})`
            }
            result += `<br>&nbsp&nbsp&nbsp&nbsp- ${this.tuesday}: ${assocWorkedDay[this.tuesdayWorkedDay]}`
            if (this.tuesdayComment !== null) {
                result += ` (${this.tuesdayComment})`
            }
            result += `<br>&nbsp&nbsp&nbsp&nbsp- ${this.wednesday}: ${assocWorkedDay[this.wednesdayWorkedDay]}`
            if (this.wednesdayComment !== null) {
                result += ` (${this.wednesdayComment})`
            }
            result += `<br>&nbsp&nbsp&nbsp&nbsp- ${this.thursday}: ${assocWorkedDay[this.thursdayWorkedDay]}`
            if (this.thursdayComment !== null) {
                result += ` (${this.thursdayComment})`
            }
            result += `<br>&nbsp&nbsp&nbsp&nbsp- ${this.friday}: ${assocWorkedDay[this.fridayWorkedDay]}`
            if (this.fridayComment !== null) {
                result += ` (${this.fridayComment})`
            }

            result += "<br><br>Je reste à ta disposition si tu as la moindre question.<br><br>Bien à toi,<br><br>Kevin Herrbach"
            return result
        },
        formatMailHref: function () {
            return "mailto:" + config.mail + "?Subject=" + this.getObject + "&Body=" +
             this.getBody.replace(/<br\s*\/?>/mg,"%0D%0A")
             .replace(/&nbsp&nbsp&nbsp&nbsp/mg, "%09")
        }
    },
    methods: {
        getWeekNumber : function (d) {
            // Copy date so don't modify original
            d = new Date(Date.UTC(d.getFullYear(), d.getMonth(), d.getDate()));
            // Set to nearest Thursday: current date + 4 - current day number
            // Make Sunday's day number 7
            d.setUTCDate(d.getUTCDate() + 4 - (d.getUTCDay()||7));
            // Get first day of year
            var yearStart = new Date(Date.UTC(d.getUTCFullYear(),0,1));
            // Calculate full weeks to nearest Thursday
            var weekNo = Math.ceil(( ( (d - yearStart) / 86400000) + 1)/7);
            // Return array of year and week number
            return weekNo;
        }
    },
    data: function () {
        let lastWeekMonday = new Date()
        lastWeekMonday.setDate(lastWeekMonday.getDate() - 7 - (lastWeekMonday.getDay() + 6) % 7)
        const year = lastWeekMonday.getFullYear()
        const week = this.getWeekNumber(lastWeekMonday)
        let options = {weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'}
        const monday =  lastWeekMonday.toLocaleDateString('fr-FR', options)
        lastWeekMonday.setDate(lastWeekMonday.getDate() + 1)
        const tuesday =  lastWeekMonday.toLocaleDateString('fr-FR', options)
        lastWeekMonday.setDate(lastWeekMonday.getDate() + 1)
        const wednesday =  lastWeekMonday.toLocaleDateString('fr-FR', options)
        lastWeekMonday.setDate(lastWeekMonday.getDate() + 1)
        const thursday =  lastWeekMonday.toLocaleDateString('fr-FR', options)
        lastWeekMonday.setDate(lastWeekMonday.getDate() + 1)
        const friday =  lastWeekMonday.toLocaleDateString('fr-FR', options)
        return {
            monday: monday,
            tuesday: tuesday,
            wednesday: wednesday,
            thursday: thursday,
            friday: friday,
            mondayWorkedDay: '1',
            tuesdayWorkedDay: '1',
            wednesdayWorkedDay: '1',
            thursdayWorkedDay: '1',
            fridayWorkedDay: '1',
            mondayComment: null,
            tuesdayComment: null,
            wednesdayComment: null,
            thursdayComment: null,
            fridayComment: null,
            dialog: false,
            week: week,
            year: year
        }
    }
}
</script>
<style lang="scss">
.home {
    &__title {
        margin-bottom: 70px;
    }
}
</style>