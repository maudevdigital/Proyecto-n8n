# Convalidaciones con n8n

## 🎯 Visión
Automatizar el proceso de convalidaciones estudiantiles usando **n8n + Google Drive + Google Sheets + Gmail**.

## 👥 Roles
- **PO (Product Owner):** Felipe A.
- **SM (Scrum Master):** Franco L.
- **Dev–Tester (pares):**
  - lucasmaulenr ⇄ Felipe Vergara R.
  - maticata0111-bit ⇄ lucasmaulenr

## ⚙️ Cómo correr n8n en Docker
```bash
docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

