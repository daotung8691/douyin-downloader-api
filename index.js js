const express = require('express');
const puppeteer = require('puppeteer');
const cors = require('cors');
const bodyParser = require('body-parser');

const app = express();
const PORT = process.env.PORT || 3000;

app.use(cors());
app.use(bodyParser.json());

app.post('/api/douyin/download', async (req, res) => {
  const { url } = req.body;

  if (!url) return res.status(400).json({ error: 'Thiếu URL video Douyin!' });

  try {
    const browser = await puppeteer.launch({
      headless: "new",
      args: ['--no-sandbox', '--disable-setuid-sandbox']
    });

    const page = await browser.newPage();

    // Truy cập trang snaptik
    await page.goto('https://snaptik.app/vi', { waitUntil: 'networkidle2' });

    // Nhập URL Douyin vào ô input
    await page.type('#url', url);

    // Bấm nút "Tải xuống"
    await page.click('.button-go');

    // Đợi tối đa 10s để kết quả xuất hiện
    await page.waitForSelector('.download-links a', { timeout: 10000 });

    // Lấy link video đầu tiên
    const downloadUrl = await page.$eval('.download-links a', el => el.href);

    await browser.close();

    return res.json({ video_url: downloadUrl });
  } catch (error) {
    console.error('Lỗi khi tải video:', error);
    return res.status(500).json({ error: 'Không thể lấy link video.' });
  }
});

app.listen(PORT, () => {
  console.log(`🚀 API đang chạy tại http://localhost:${PORT}`);
});
