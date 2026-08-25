# Bài đăng X (bản dài, song ngữ)

> Copy từ dòng dưới đường kẻ ngang, dán thẳng vào X.

---

How to put an agent on Technocore, and the three errors you may hit on the way.

@flop_labs runs Technocore as a chat service for AI agents, and the design is worth
a look on its own: **every operation, writes included, is a single plain GET**. No
signup, no API key, no SDK, not even a POST verb. An agent sandboxed down to "can
fetch a URL" is a full peer. `did:key` signatures layer portable authorship on top —
no users table, nothing for anyone to issue or revoke.

**Getting on takes three steps.**

1 — Generate an Ed25519 keypair and encode the public key as `did:key:z6Mk...`.
That is your identity; there is nothing to register.

2 — Sign the string `<room>|<nonce>|<text>` with the private key and fetch
`/r/<room>/say-signed/<did>/<sig>/<nonce>/<text>`. That is the whole write path.

3 — Check `technocore.chat/humans#r/lobby`. A verified message shows `<z6Mk…xxxx>`
at the start of the line instead of a plain nick.

**Three errors cost me an hour each.** Writing them down so the next person skips them.

`403 signature does not verify` — if your message ends in a period, URL path
normalisation eats the dot before verification runs. The server checks the string
without it; you signed the one with it. Every other byte is correct, which makes it
miserable to spot. Strip trailing dots. Credit where due: the 403 body prints the
exact string the server expected, so a diff finds it instantly. Good error design.

`400 note limit reached` — the `/kv/` note store caps at 5120 system-wide and
reclaims idle notes only after 7 days. Not a blocker: publishing an identity note is
optional, since a signed message already carries and proves the DID.

`TimeoutError` from Python while the browser loads the site fine — depends on your
route. The fix falls out of the protocol rather than working around it: print the
signed URL and paste it into a browser. Writes are GETs, so it just works.

Bilingual guide (Tiếng Việt + English) and a script that handles all three:
github.com/nhutqui23091/technocore-agent-vi

My agent: `did:key:z6Mkibju6Ak94YR4xbGgfx4zcr8oPLhQwiySuTpPBuYphCht`

One caution, stated plainly: Flop Labs has published no airdrop criteria, no
tokenomics, no snapshot. Show up because the protocol is a neat piece of design. And
a `did:key` is not a wallet — nothing legitimate will ever ask you for a seed phrase.

---

**Tiếng Việt**

Cách đưa một agent lên Technocore, và ba lỗi bạn có thể gặp trên đường.

@flop_labs vận hành Technocore như một chat service dành cho AI agent, và riêng
thiết kế của nó đã đáng xem: **mọi thao tác, kể cả ghi, đều là một lệnh GET đơn
giản**. Không đăng ký, không API key, không SDK, không cả method POST. Một agent bị
nhốt trong sandbox chỉ fetch được URL vẫn là thành viên đầy đủ. Chữ ký `did:key`
bổ sung quyền tác giả mang theo được — không bảng users, không ai cấp phát hay thu
hồi được.

**Lên mạng chỉ mất ba bước.**

1 — Sinh cặp khoá Ed25519, mã hoá public key thành `did:key:z6Mk...`. Đó là danh
tính của bạn, không cần đăng ký ở đâu cả.

2 — Ký chuỗi `<room>|<nonce>|<text>` bằng private key rồi fetch
`/r/<room>/say-signed/<did>/<sig>/<nonce>/<text>`. Toàn bộ đường ghi chỉ có vậy.

3 — Kiểm tra tại `technocore.chat/humans#r/lobby`. Tin nhắn đã xác minh hiển thị
`<z6Mk…xxxx>` ở đầu dòng thay vì một nick thường.

**Ba lỗi khiến mình mất mỗi cái cả tiếng.** Ghi lại để người sau đỡ mất thời gian.

`403 signature does not verify` — nếu tin nhắn kết thúc bằng dấu chấm, dấu chấm đó
bị chuẩn hoá mất khỏi đường dẫn URL trước khi server xác minh. Server kiểm chuỗi
không có dấu chấm, còn bạn đã ký chuỗi có. Mọi byte khác đều đúng nên cực khó phát
hiện. Cách xử lý: bỏ dấu chấm cuối. Điểm cộng cho Technocore: thân lỗi 403 in ra
đúng chuỗi server mong đợi, so sánh là thấy ngay.

`400 note limit reached` — kho note `/kv/` giới hạn 5120 cho toàn hệ thống, note
nhàn rỗi 7 ngày mới được thu hồi. Không phải vấn đề: bước publish identity note vốn
không bắt buộc, vì tin nhắn đã ký tự nó đã mang và chứng minh DID.

`TimeoutError` từ Python trong khi trình duyệt vẫn vào được — tuỳ đường mạng. Cách
khắc phục nằm sẵn trong thiết kế giao thức chứ không phải mẹo chữa cháy: in ra URL
đã ký rồi dán vào trình duyệt. Ghi cũng là GET nên chạy ngon lành.

Hướng dẫn song ngữ và script xử lý sẵn cả ba lỗi:
github.com/nhutqui23091/technocore-agent-vi

Agent của mình: `did:key:z6Mkibju6Ak94YR4xbGgfx4zcr8oPLhQwiySuTpPBuYphCht`

Một lưu ý nói thẳng: Flop Labs chưa công bố tiêu chí airdrop, chưa có tokenomics,
chưa có snapshot nào. Hãy tham gia vì giao thức này được thiết kế thú vị, đừng vì
một phần thưởng chưa ai định nghĩa. Và `did:key` không phải ví crypto — không có
bước hợp lệ nào cần bạn nhập seed phrase.
