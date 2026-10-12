 import os
import discord
from discord.ext import commands

# 1. Cấu hình quyền hạn (Intents) bắt buộc để bot đọc được tin nhắn
intents = discord.Intents.default()
intents.message_content = True  # Quan trọng: Cho phép bot đọc nội dung chat

# 2. Khởi tạo bot với tiền tố lệnh là dấu chấm '!'
bot = commands.Bot(command_prefix="!", intents=intents)

# Sự kiện: Bot thông báo khi đã khởi động xong
@bot.event
async def on_ready():
    print(f"🤖 Bot đã sẵn sàng với tên: {bot.user}")

# Sự kiện: Tự động lắng nghe mọi tin nhắn được gửi lên server
@bot.event
async def on_message(message):
    # Đảm bảo bot không tự trả lời tin nhắn của chính nó (tránh lặp vô tận)
    if message.author == bot.user:
        return

    # Chuyển nội dung tin nhắn về chữ thường để không bị lỗi viết hoa/thường
    noi_dung = message.content.lower()

    # Kiểm tra nếu trong câu chat có chứa từ khóa "hello bot"
    if "hello bot" in noi_dung:
        # Gửi lời chào kèm theo tên của người vừa nhắn tin
        await message.channel.send(f"Xin chào {message.author.mention}! Chúc bạn một ngày tuyệt vời nhé 👋")

    # Dòng này rất quan trọng: Giúp các lệnh dạng !chao, !gia (nếu có) vẫn hoạt động bình thường
    await bot.process_commands(message)

# 3. Chạy bot bằng Token của bạn
# (Nếu bạn lưu token trong biến môi trường hoặc file bảo mật, hãy gọi nó ra đây)
TOKEN = os.environ.get("DISCORD_TOKEN") 
# Hoặc nếu chạy trên máy tính cá nhân, bạn có thể dán trực tiếp token vào đây:
# TOKEN = "MÃ_TOKEN_CỦA_BẠN_Ở_ĐÂY"

bot.run(TOKEN)
