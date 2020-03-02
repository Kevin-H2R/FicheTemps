<template>
    <v-card>
        <v-card-title>Email pour Bébert</v-card-title>
        <v-card-text>
            <v-container>
                <v-row align="center">
                    <v-col cols="10">
                        {{ object }}
                    </v-col>
                    <v-col cols="2">
                        <v-btn fab icon x-small color="primary"
                            v-clipboard:copy="object"
                        >
                            <v-icon>mdi-content-copy</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-card-text>
        <v-card-text v-html="body">
        </v-card-text>
        <v-card-actions>
            <v-spacer/>
            <v-btn fab icon big color="primary"
            v-clipboard:copy="formattedData"
            >
                <v-icon>mdi-content-copy</v-icon>
            </v-btn>
            <v-spacer/>
        </v-card-actions>
    </v-card>
</template>
<script>
export default {
    name: 'my-mail',
    props: {
        object: {
            type: String,
            required: true
        },
        body: {
            type: String,
            required: true
        }
    },
    data: function () {
        const formattedData = this.body.replace(/<br\s*\/?>/mg,"\n").replace(/&nbsp&nbsp&nbsp&nbsp/mg, "\t")
        return {
            formattedData: formattedData
        }
    }
}
</script>