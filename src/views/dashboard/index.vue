<template>
  <div class="dashboard-container">
    <div class="dashboard-text">name:{{name}}</div>
    <div class="dashboard-text">roles:<span v-for='role in roles' :key='role'>{{role}}</span></div>
    <div class="dashboard-text"><span v-for='role in roles' :key='role'>{{felix}}</span></div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
const {EchoRequest,
  ServerStreamingEchoRequest} = require('../../proto/echo_pb.js');
const {EchoServiceClient,EchoServicePromiseClient} = require('../../proto/echo_grpc_web_pb.js');


const AEPB = require('../../proto/adverse_event_pb');
const {AdverseEventPromiseClient} = require('../../proto/adverse_event_grpc_web_pb');
const grpc = {};
grpc.web = require('grpc-web');

export default {
  name: 'dashboard',
  computed: {
    ...mapGetters([
      'name',
      'roles',
        "felix"
    ])
  },
    mounted(){
        var adverseEventService =new AdverseEventPromiseClient('http://106.14.125.33:10000', null, null);

        var RepAdverseEventView = new AEPB.RepAdverseEventView()
        RepAdverseEventView.setId(1)

        var self = this;
        var headers={
            "custom-header-1": "value1"
        }

        adverseEventService.view(RepAdverseEventView, headers).then(function(res){
            var code = res.getCode();
            var Msg = res.getMsg();
            var datainfo = res.getDataInfoList();



            console.log(code,Msg);
            console.log("datainfo",datainfo[0].getDomainName());
        })

    }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
