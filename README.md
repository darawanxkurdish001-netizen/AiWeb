import telebot

TOKEN = "8338253518:AAGTtAere5kFU1ZDCPKqXNXOx1G7KOqkN7U"
bot = telebot.TeleBot(TOKEN)

@bot.message_handler(content_types=['new_chat_members'])
def welcome(message):
    group_name = message.chat.title
    
    for user in message.new_chat_members:
        name = user.first_name
        mention = f"[{name}](tg://user?id={user.id})"
        members = bot.get_chat_members_count(message.chat.id)
        
        text = f"""┏•━•━•━•━◎ ━•━•━•━◎•━•━┓
💙┋بەخێربێیت بۆ گرووپی
{group_name}
┍┄┄┄┄༻❀༺┄┄┄┄┑
❤┋ناووی بەڕێزت لە گرووپ
{name}
┍┄┄┄┄༻❀༺┄┄┄┄┑
🖤┋هاشتاکی بەڕێزت لە گرووپ
{mention}
┍┄┄┄┄༻❀༺┄┄┄┄┑
💛┋بەڕێزت کەسی ژمارە
{members}
┏━━━━━━◎──────┓"""
        
        bot.send_message(message.chat.id, text, parse_mode="Markdown")

bot.polling()
