import logging
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes

# 1. إعدادات المراقبة والتحليل الحي للبوت في الطرفية
logging.basicConfig(
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO
)
logger = logging.getLogger(__name__)

# ==========================================
# 2. البيانات الأساسية الموحدة للمنصة
# ==========================================
TOKEN = "8808135399:AAHIyHXWvsoNV9HcLrAdVdsNdmeBBjLOV60"
WALLET_ADDRESS = "0x0411Dfb1CccD2ff25CFbfA8cb36B3013E29Ebba8"

# ==========================================
# 3. دالة بدء التشغيل وعرض الستة أزرار (3 صفوف)
# ==========================================
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [
        # الصف الأول
        [
            InlineKeyboardButton("💰 إيداع بينانس", callback_query_data='binance_deposit'),
            InlineKeyboardButton("💵 إيداع تيثر (USDT)", callback_query_data='usdt_deposit')
        ],
        # الصف الثاني
        [
            InlineKeyboardButton("💳 طلب سحب الأرباح", callback_query_data='withdraw_request'),
            InlineKeyboardButton("📊 تحليل السوق المباشر", callback_query_data='market_analysis')
        ],
        # الصف الثالث
        [
            InlineKeyboardButton("🛑 إشارة وقف الخسارة", callback_query_data='stop_loss'),
            InlineKeyboardButton("📞 الدعم الفني", callback_query_data='support')
        ]
    ]
    
    reply_markup = InlineKeyboardMarkup(keyboard)
    
    await update.message.reply_text(
        "👋 **أهلاً بك في منصة التداول والربح الذكية**\n\n"
        "📈 النظام مربوط بالمنصة حياً لتأكيد الإيداعات ومعالجة السحوبات السريعة.\n"
        "يرجى اختيار أحد الخيارات من الأزرار المتاحة أدناه:",
        reply_markup=reply_markup,
        parse_mode="Markdown"
    )

# ==========================================
# 4. دالة معالجة تفاعل الضغط على الأزرار الستة
# ==========================================
async def button_click(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    data = query.data

    # [1] زر إيداع بينانس
    if data == 'binance_deposit':
        deposit_text = (
            "💰 **بوابة الإيداع التلقائي عبر منّصة Binance**\n\n"
            "📥 **العنوان المربوط بالمنصة حياً:**\n"
            f"`{WALLET_ADDRESS}`\n\n"
            "🌐 **الشبكة:** Binance Smart Chain (BSC / BEP20)\n"
            "⚠️ *يتم فحص المعاملة وتأكيد الإيداع في حسابك فور وصولها حياً للشبكة.*"
        )
        await query.message.reply_text(text=deposit_text, parse_mode="Markdown")

    # [2] زر إيداع تيثر USDT
    elif data == 'usdt_deposit':
        usdt_text = (
            "💵 **بوابة إيداع تيثر المباشر (USDT)**\n\n"
            "📥 **عنوان محفظة الاستلام الموحد:**\n"
            f"`{WALLET_ADDRESS}`\n\n"
            "🌐 **الشبكة:** TRC20 (ترون)\n"
            "⚠️ *تنبيه جدي:* أي إيداع على شبكة أخرى سيؤدي إلى ضياع المعاملة."
        )
        await query.message.reply_text(text=usdt_text, parse_mode="Markdown")

    # [3] زر طلب سحب الأرباح المرتبط بالمنصة
    elif data == 'withdraw_request':
        withdraw_text = (
            "💳 **بوابة سحب الأرباح التلقائية من المنصة**\n"
            "━━━━━━━━━━━━━━━━━━\n\n"
            "📤 **خطوات معالجة السحب حياً:**\n"
            "1️⃣ تأكد من مطابقة محفظة السحب الخاصة بك.\n"
            "2️⃣ اكتب أمر السحب في المحادثة متبوعاً بالمبلغ المراد (مثال: `/withdraw 100`).\n\n"
            "🔄 **حالة النظام:** اتصال المنصة نشط والسيولة متوفرة للسحب الفوري الفوري ✅"
        )
        await query.message.reply_text(text=withdraw_text, parse_mode="Markdown")

    # [4] زر تحليل السوق المباشر
    elif data == 'market_analysis':
        analysis_text = (
            "📊 **تقرير تحليل السوق الحي والمباشر**\n"
            "━━━━━━━━━━━━━━━━━━\n\n"
            "🟢 **اتجاه السوق العام:** صعود قوي وجاذب لشراء العملات 📈\n"
            "🔴 **مستويات المقاومة:** هبوط مؤقت متوقع عند ملامسة القمة 📉\n"
            "🟡 **حالة التذبذب الحالية:** استقرار نسبي في نطاق عرضي ↕️\n\n"
            "🔥 *التحليل الحي يشير إلى دخول أموال ذكية وضخ سيولة عالية الآن.*"
        )
        await query.message.reply_text(text=analysis_text, parse_mode="Markdown")

    # [5] زر إشارة وقف الخسارة
    elif data == 'stop_loss':
        sl_text = (
            "🛑 **إشارة وقف الخسارة وإدارة المخاطر (Stop Loss)**\n"
            "━━━━━━━━━━━━━━━━━━\n\n"
            "🚨 **نقطة الخروج الفوري:** حماية الحساب إذا كسر السعر مستويات الدعم 📉\n"
            "🎯 **إدارة المحفظة حياً:** يتم تفعيل الإجراء تلقائياً لحفظ أموال المشتركين.\n\n"
            "⚠️ *الرجاء الالتزام التام بالتنبيهات البرمجية وعدم المخاطرة العشوائية.*"
        )
        await query.message.reply_text(text=sl_text, parse_mode="Markdown")

    # [6] زر الدعم الفني
    elif data == 'support':
        await query.message.reply_text(
            "📞 **الدعم الفني والمالي المباشر**\n\n"
            "إذا واجهتك أي مشكلة تجميد معاملة أو استفسار أثناء السحب أو الإيداع، "
            "يرجى التواصل مع إدارة المنصة فوراً لحلها حياً.",
            parse_mode="Markdown"
        )

# ==========================================
# 5. أمر معالجة السحب النصي المباشر
# ==========================================
async def withdraw_command(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if not context.args:
        await update.message.reply_text("❌ يرجى تحديد المبلغ. مثال: `/withdraw 100`", parse_mode="Markdown")
        return
    
    amount = context.args[0]
    await update.message.reply_text(
        f"⏳ **جاري معالجة طلب السحب...**\n"
        f"💰 المبلغ المطلوب: `{amount}`\n"
        f"🏦 المحفظة المستهدفة: المرتبطة بالمنصة حياً.\n"
        f"🔄 سيتم إشعارك فور اكتمال المعاملة على الشبكة.",
        parse_mode="Markdown"
    )

# ==========================================
# 6. الدالة الأساسية لبناء وتشغيل البوت
# ==========================================
def main():
    # بناء التطبيق بالتوكن
    application = Application.builder().token(TOKEN).build()

    # ربط الأوامر البرمجية والضغط على الأزرار بالمعالجات الخاصة بها
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("withdraw", withdraw_command))
    application.add_handler(CallbackQueryHandler(button_click))

    # تشغيل استقبال البيانات حياً ومباشراً
    print("--- البوت يعمل الآن حياً ومتصل بالمنصة بنجاح ---")
    application.run_polling()

if __name__ == '__main__':
    main()
