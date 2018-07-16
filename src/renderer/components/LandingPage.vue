<template>
    <div class="container d-flex flex-row h-100" id="wrapper">
        <div class="w-100 align-self-center">
            <div class="row">
                <div class="col-md-12 text-center mt-3">
                    <h1>WhatArmy OTS</h1>
                </div>
            </div>
            <div class="row" v-if="shared_link != ''">
                <div class="col-md-12 text-center">
                    <h4 class="text-success">{{shared_link}}</h4>
                    <p>
                        <small class="text-muted">Url to you secret has been automatically added to your clipboard.</small>
                    </p>
                </div>
            </div>
            <div class="row mt-3">
                <div class="col-md-12">
                    <textarea v-model="secret" cols="30" rows="10" class="form-control"></textarea>
                </div>
            </div>
            <div class="row mt-3">
                <div class="col-md-12 text-center">
                    <button @click.prevent="makeSecret()" class="btn btn-success btn-lg">Share secret</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import SystemInformation from './LandingPage/SystemInformation'

    const {clipboard} = require('electron');

    export default {
        name: 'landing-page',
        components: {SystemInformation},
        data() {
            return {
                secret: '',
                shared_link: '',
            }
        },
        methods: {
            makeSecret: function () {
                const querystring = require('querystring');
                var t = this;
                t.shared_link = '';
                this.$http.post('https://ots.whatarmy.com/api/v1/share', querystring.stringify({secret: t.secret})).then(function (response) {
                    t.shared_link = 'https://ots.whatarmy.com/secret/' + response.data.secret_key;
                    clipboard.writeText(t.shared_link, 'selection');

                }).catch(function (error) {
                    //error
                });
            },
            open(link) {
                this.$electron.shell.openExternal(link)
            }
        }
    }
</script>

<style>
    body{
        background: radial-gradient(
                ellipse at top left,
                rgba(255, 255, 255, 1) 40%,
                rgba(229, 229, 229, .9) 100%
        );
    }
    html,body,#app{
        min-height: 100vh;
    }
    @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family: 'Source Sans Pro', sans-serif;
    }

    #wrapper {
        height: 100vh;
        padding: 60px 80px;
        width: 100vw;
    }

    #logo {
        height: auto;
        margin-bottom: 20px;
        width: 420px;
    }

    main {
        display: flex;
        justify-content: space-between;
    }

    main > div {
        flex-basis: 50%;
    }

    .left-side {
        display: flex;
        flex-direction: column;
    }

    .welcome {
        color: #555;
        font-size: 23px;
        margin-bottom: 10px;
    }

    .title {
        color: #2c3e50;
        font-size: 20px;
        font-weight: bold;
        margin-bottom: 6px;
    }

    .title.alt {
        font-size: 18px;
        margin-bottom: 10px;
    }

    .doc p {
        color: black;
        margin-bottom: 10px;
    }

    .doc button {
        font-size: .8em;
        cursor: pointer;
        outline: none;
        padding: 0.75em 2em;
        border-radius: 2em;
        display: inline-block;
        color: #fff;
        background-color: #4fc08d;
        transition: all 0.15s ease;
        box-sizing: border-box;
        border: 1px solid #4fc08d;
    }

    .doc button.alt {
        color: #42b983;
        background-color: transparent;
    }
</style>
