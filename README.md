# nats-to-redpanda
consume messages from nats core or nats jetstream to redpanda topics


# Steps to run
## Start nats server without jet stream
docker run -p 4222:4222 nats -js

## Start nats server with jet stream
docker run -p 4222:4222 -v nats:/data nats -js -sd /data

NATS_URL="${NATS_URL:-nats://localhost:4222}"

## Download nats cli to pub/sub messages
download nats-0.2.2-darwin-arm64.zip
unzip nats-0.2.2-darwin-arm64.zip
ensure `nats` is able to run on mac

## get info on nats server
nats account info

## create nats stream
nats stream add my_stream

## get info abt stream you created
nats stream info my_stream

## publish 1000 messages to subject foo
nats pub foo --count=1000 --sleep 1s "publication #{{.Count}} @ {{.TimeStamp}}"

## publish 1 message
nats pub foo "hello"

## start redpanda cluster
docker compose -f single-node-rp-cluster.yaml up -d

## create and consume topic
rpk profile set brokers=localhost:19092
rpk topic produce testtopic
rpk topic consume testtopic

## start redpanda connect pipeline
rpk connect run rpcn-nats-core-to-rpd.yaml &
rpk connect run rpcn-nats-jtstrm-to-rpd.yaml &

## docs
### nats core and nats jetstream
https://docs.nats.io/nats-concepts/core-nats/pubsub/pubsub_walkthrough
https://docs.nats.io/nats-concepts/jetstream/js_walkthrough

###publish
https://natsbyexample.com/examples/messaging/pub-sub/cli
https://docs.nats.io/nats-concepts/jetstream/js_walkthrough


# docs that dont work
#https://github.com/nats-io/natscli?tab=readme-ov-file#installation
#https://docs.nats.io/using-nats/nats-tools/nats_cli
# to install nats tools - doesnt work
brew tap nats-io/nats-tools
brew install nats-io/nats-tools/nats

