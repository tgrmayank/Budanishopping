BUDANI SHOPPING — IMAGE HOSTING PACKAGE

KYA BADLA HAI
- Existing HTML design, pages, navigation aur shopping flow ko same rakha gaya hai.
- HTML ke andar embedded base64 image data ko assets/ folder ki normal WebP/PNG files mein nikala gaya hai.
- Admin panel se naye product images upload karne par image Supabase Storage par jayegi; product record mein public image URL save hoga, base64 nahi.
- Upload par image resize/compress hoti hai (WebP) aur 12 MB limit hai.

SUPABASE SETUP (REQUIRED FOR ADMIN UPLOAD)
1. https://supabase.com par project banao.
2. Storage > New bucket: product-images. Bucket ko Public ON karo.
3. SQL Editor mein neeche wali policy run karo (testing ke liye public uploads; production mein admin authentication ke saath restrict karna zaroori hai):

create policy "public product image uploads" on storage.objects for insert to anon with check (bucket_id = 'product-images');
create policy "public product image reads" on storage.objects for select to anon using (bucket_id = 'product-images');

4. Project Settings > API se Project URL aur anon/public key lo. index.html mein SB_URL aur SB_ANON_KEY replace karo. SB_BUCKET product-images hi rakho.
5. Hosting par poora folder upload karo, sirf index.html nahi — assets folder bhi upload karna zaroori hai.

IMPORTANT SECURITY
- Supabase anon key browser mein visible hoti hai; ye normal hai, lekin Storage policies ko carefully limit karo. Upar INSERT policy public testing ke liye hai aur koi bhi upload kar sakta hai. Real store ke liye authenticated admin + restrictive RLS policy ya Edge Function use karo.
- GitHub raw images public read/CDN ke liye theek hai, lekin browser se GitHub repo mein secure upload karne ke liye personal access token HTML mein rakhna unsafe hai. Isliye product uploads ke liye Supabase choose kiya.
- Current order/config cloud sync ka CLOUD endpoint original file mein blank tha; is package ne us existing order backend ko configure nahi kiya. Product sync bhi tabhi work karega jab original CLOUD REST endpoint configure ho. Supabase yahan image storage ke liye add kiya hai.
- Live Supabase upload ko credentials ke bina test nahi kiya ja sakta. Placeholder values ko apne project se fill karna hoga.

LOCAL PREVIEW
index.html ko browser mein open karo. assets/ folder same location par hona chahiye.
