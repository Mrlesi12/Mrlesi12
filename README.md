name = "krestiki-noliki"
version = "0.1.0"
edition = "2021"

[dependencies]
serenity = { git = "https://github.com/serenity-rs/serenity.git", rev = "ba3be69166f54c5986e4cc9438bc5bb4606fa4c2", default-features = false, features = ["builder", "cache", "client", "model", "utils", "gateway", "rustls_backend"] }
tokio = { version = "1.22", features = ["rt-multi-thread"]
use serenity::async_trait;
use serenity::all::{Message, Ready};
use serenity::prelude::*;

struct Handler;

#[async_trait]
impl EventHandler for Handler {
    async fn message(&self, ctx: Context, msg: Message) {
        if msg.content == "!ping" {
            if let Err(err) = msg.reply(&ctx.http, "pong!").await {
                eprintln!("Error: {err}");
            }
        }
    }

    async fn ready(&self, _: Context, ready: Ready) {
        println!("{} has connected!", ready.user.name);
    }
}

#[tokio::main]
async fn main() {
    let intents = GatewayIntents::GUILD_MESSAGES
        | GatewayIntents::MESSAGE_CONTENT;

    let mut client = Client::builder(include_str!("./../token.txt"), intents)
        .event_handler(Handler)
        .await
        .expect("Failed to create client!");

    if let Err(err) = client.start().await {
        eprintln!("Client error: {err:?}");
    }
}
