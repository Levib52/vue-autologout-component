<template>
    <div v-if"warningZone">
    <!-- display any warning message you choose to let the user know they are about to be logged out due to inactivity -->
        <h2>You are about to be logged out due to inactivity.</h2>
    </div>
</template>

<script>
    export default {
        name: "AutoLogout",

        data: function () {
            return {
                //check for any user activity with events. add any events you choose
                events: ['click', 'mousemove', 'mousedown', 'scroll', 'keypress', 'load'],
                //declare the variables for warning timer, logout timer, and warning "zone"
                warningTimer: null,
                logoutTimer: null,
                warningZone: false,

            }
        },

        mounted() {
            //loop through the events array. when any events occur, evoke the reset timer method which clears the timeout
            this.events.foreach(function(event){
                window.addEventListener(event, this.resetTimer);
            }, this);

            this.setTimers();
        },

        destroyed() {
            //remove event listeners
            this.events.foreach(function(event){
                window.removeEventListener(event, this.resetTimer);
            }, this);

            this.resetTimer();
        },

        methods: {
            setTimers: function () {
                //This timer will countdown to the warning message if there is no user activity. It is set for 14 minutes.
                this.warningTimer = setTimeout(this.warningMessage, 14 * 60 *1000);
                //This timer will countdown to log the user out if there is no user activity. It is set for 15 minutes.
                this.warningTimer = setTimeout(this.logoutUser, 15 * 60 *1000);
            
                this.warningZone = false;
            },

            warningMessage: function () {
                //warn the user they are about to be logged out with a message.
                this.warningZone = true;
            },

            logoutUser: function () {
                //this method will log the user out. Add any logic you need to log the user out.
                document.getElementById('logout-form').submit();
            },

            resetTimer: function () {
                //this will reset the timers with any user activity.
                clearTimeout(this.warningTimer);
                clearTimeout(this.logoutTimer);

                this.setTimers();
            }
        }
    }
</script>

<style scoped>
</style>