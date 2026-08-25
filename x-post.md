# Bài đăng X (bản dài, song ngữ)

> Thay `nhutqui2309` trong link GitHub bằng username thật của bạn nếu khác.
> Copy từ dòng dưới đường kẻ, dán thẳng vào X.

---

Onboarding an agent to Technocore: three things the spec doesn't warn you about.

I set up a did:key agent on @flop_labs' Technocore this week. Three walls cost me an hour each. Writing them down so the next person skips them.

**1. A trailing period breaks your signature.**

Sign `lobby|<nonce>|…any SDK could.` and the server verifies `…any SDK could` — URL path normalisation eats the final dot before verification runs. You get `403 signature does not verify` with every other byte correct, which is a miserable thing to debug. Strip trailing dots before you sign.

Credit where due: the 403 body prints the exact string the server expected. Diff it against what you signed and the difference jumps out. That is good error design.

**2. The note store fills up.**

Publishing an identity note to `/kv/did/<fingerprint>` can return `400 note limit reached` — 5120 notes system-wide, idle ones reclaimed after 7 days. Not a blocker: the step is optional. A signed message already carries the DID and proves it.

**3. Python may time out where the browser doesn't.**

Depending on your route, urllib hangs while Chrome loads the site fine. The fix falls out of the protocol itself: print the signed URL, paste it into a browser. Every write is a plain GET, so it just works.

That last point is the whole design, not a workaround. Writes are GETs, so an agent sandboxed down to "can fetch a URL" is a full peer — no signup, no API key, no SDK, no POST verb. `did:key` signatures layer portable authorship on top: no users table, nothing for anyone to issue or revoke.

Bilingual guide (Tiếng Việt + English) and script:
github.com/nhutqui2309/technocore-agent-vi

My agent: `did:key:z6Mkibju6Ak94YR4xbGgfx4zcr8oPLhQwiySuTpPBuYphCht`

One caution, stated plainly: Flop Labs has published no airdrop criteria, no tokenomics, no snapshot. Show up because the protocol is a genuinely neat piece of design. And a `did:key` is not a wallet — nothing legitimate will ever ask you for a seed phrase.

---

**Tiếng Việt**

Đưa agent lên Technocore: ba cái bẫy mà tài liệu không nói.

Tuần này mình dựng một agent `did:key` trên Technocore của @flop_labs. Ba chỗ vướng, mỗi chỗ mất cả tiếng. Ghi lại để người sau đỡ mất thời gian.

**1. Dấu chấm cuối câu làm hỏng chữ ký.**

Bạn ký chuỗi kết thúc bằng `…any SDK could.` nhưng server lại xác minh chuỗi `…any SDK could` — dấu chấm cuối bị chuẩn hoá mất khỏi đường dẫn URL trước khi tới bước xác minh. Kết quả là `403 signature does not verify` trong khi mọi thứ khác đều đúng. Cực khó đoán. Cách xử lý: bỏ dấu chấm cuối trước khi ký.

Điểm cộng cho Technocore: thông báo 403 in ra đúng chuỗi mà server mong đợi, so sánh với chuỗi mình đã ký là thấy ngay khác chỗ nào.

**2. Kho note có thể đầy.**

Bước publish identity note vào `/kv/did/<fingerprint>` có thể trả về `400 note limit reached` — toàn hệ thống giới hạn 5120 note, note nhàn rỗi 7 ngày mới được thu hồi. Không sao cả: bước này không bắt buộc, vì tin nhắn đã ký tự nó đã mang và chứng minh DID rồi.

**3. Python có thể timeout trong khi trình duyệt vẫn vào được.**

Tuỳ đường mạng, urllib treo còn Chrome mở bình thường. Cách khắc phục nằm sẵn trong thiết kế giao thức: in ra URL đã ký rồi dán vào trình duyệt. Mọi thao tác ghi đều là GET nên chạy ngon lành.

Và đó chính là điểm hay nhất của Technocore chứ không phải mẹo chữa cháy. Ghi cũng là GET, nên một agent bị nhốt trong sandbox chỉ fetch được URL vẫn là thành viên đầy đủ — không đăng ký, không API key, không SDK, không cả method POST. Chữ ký `did:key` bổ sung quyền tác giả mang theo được: không bảng users, không ai cấp phát hay thu hồi được.

Hướng dẫn song ngữ và script:
github.com/nhutqui2309/technocore-agent-vi

Agent của mình: `did:key:z6Mkibju6Ak94YR4xbGgfx4zcr8oPLhQwiySuTpPBuYphCht`

Một lưu ý nói thẳng: Flop Labs chưa công bố tiêu chí airdrop, chưa có tokenomics, chưa có snapshot nào. Hãy tham gia vì giao thức này được thiết kế thú vị, đừng vì một phần thưởng chưa ai định nghĩa. Và `did:key` không phải ví crypto — không có bước hợp lệ nào cần bạn nhập seed phrase.
