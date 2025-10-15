# Electrical-Standard
all electrical standard
import streamlit as st
import pandas as pd

st.set_page_config(page_title="استانداردهای برق", layout="wide")

st.title("🎯 نرم‌افزار استانداردهای برق")
st.markdown("---")

# داده‌های کامل‌تر استانداردها
standards = [
    {"استاندارد": "IEC 60364", "عنوان": "تأسیسات الکتریکی ساختمان‌ها", "گروه": "تأسیسات", "کاربرد": "طراحی سیستم‌های برق ساختمان"},
    {"استاندارد": "IEC 61140", "عنوان": "حفاظت در برابر شوک الکتریکی", "گروه": "ایمنی", "کاربرد": "ایمنی برق و حفاظت"},
    {"استاندارد": "IEEE 80", "عنوان": "سیستم‌های ارت پست‌ها", "گروه": "ارتینگ", "کاربرد": "سیستم‌های زمین‌سازی"},
    {"استاندارد": "IEC 60947", "عنوان": "قطعات فرمان و کنترل", "گروه": "قطعات", "کاربرد": "کنترل موتورها و مدارهای فرمان"},
    {"استاندارد": "IEC 60034", "عنوان": "ماشین‌های الکتریکی دوار", "گروه": "ماشین‌ها", "کاربرد": "موتورها و ژنراتورهای الکتریکی"},
    {"استاندارد": "IEC 60204", "عنوان": "ایمنی ماشین‌آلات", "گروه": "ایمنی", "کاربرد": "ماشین‌آلات صنعتی"},
    {"استاندارد": "IEC 61439", "عنوان": "تابلوهای فشار ضعیف", "گروه": "تابلوها", "کاربرد": "تابلوهای توزیع و کنترل"},
]

df = pd.DataFrame(standards)

# جستجو
col1, col2 = st.columns([2, 1])
with col1:
    search_term = st.text_input("🔍 جستجو:", placeholder="مثال: IEC یا ایمنی یا ارت...")
with col2:
    category = st.selectbox("فیلتر گروه:", ["همه"] + list(df['گروه'].unique()))

# فیلتر کردن
filtered_df = df
if search_term:
    filtered_df = filtered_df[
        filtered_df['استاندارد'].str.contains(search_term, case=False, na=False) |
        filtered_df['عنوان'].str.contains(search_term, case=False, na=False) |
        filtered_df['گروه'].str.contains(search_term, case=False, na=False) |
        filtered_df['کاربرد'].str.contains(search_term, case=False, na=False)
    ]

if category != "همه":
    filtered_df = filtered_df[filtered_df['گروه'] == category]

# نمایش نتایج
st.subheader(f"📋 نتایج جستجو ({len(filtered_df)} مورد)")
if len(filtered_df) > 0:
    st.dataframe(filtered_df, use_container_width=True, hide_index=True)
else:
    st.warning("❌ موردی یافت نشد")

# sidebar
with st.sidebar:
    st.header("ℹ️ راهنمای استفاده")
    st.info("""
    **راهنمای جستجو:**
    - شماره استاندارد (IEC, IEEE)
    - کلمات کلیدی فارسی
    - نوع استاندارد
    """)
    
    st.header("📊 آمار")
    st.metric("تعداد استانداردها", len(standards))
