const { google } = require('googleapis');

const SHEET_ID = '1QURgFIPLuVQvKSsVHa1_iT7juOKRnkCgz7oAX3pfKN0';
const SHEET_NAME = 'Deliveries';

function getAuth() {
  const creds = JSON.parse(process.env.GOOGLE_SERVICE_ACCOUNT_KEY);
  return new google.auth.GoogleAuth({
    credentials: creds,
    scopes: ['https://www.googleapis.com/auth/spreadsheets'],
  });
}

exports.handler = async (event) => {
  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type',
    'Content-Type': 'application/json',
  };

  if (event.httpMethod === 'OPTIONS') return { statusCode: 200, headers, body: '' };

  try {
    const auth = getAuth();
    const sheets = google.sheets({ version: 'v4', auth });

    // ===== GET: ดึงรายการทั้งหมด =====
    if (event.httpMethod === 'GET') {
      const res = await sheets.spreadsheets.values.get({
        spreadsheetId: SHEET_ID,
        range: `${SHEET_NAME}!A2:Z`,
      });
      const rows = res.data.values || [];
      const records = rows.map(r => ({
        id:              r[0]  || '',
        createdAt:       r[1]  || '',
        docNumber:       r[2]  || '',
        date:            r[3]  || '',
        customerCode:    r[4]  || '',
        customerName:    r[5]  || '',
        jobNumber:       r[6]  || '',
        jobName:         r[7]  || '',
        rev:             r[8]  || '00.',
        boilerType:      r[9]  || '-',
        brand:           r[10] || '-',
        capacity:        r[11] || '-',
        mfgYear:         r[12] || '-',
        fuel:            r[13] || '-',
        workingPressure: r[14] || '-',
        scope:           r[15] || '',
        engineer:        r[16] || '',
        photoCount:      r[17] || '0',
        photos:          r[18] ? JSON.parse(r[18]) : [],
      }));
      return { statusCode: 200, headers, body: JSON.stringify(records) };
    }

    // ===== POST: บันทึกใบใหม่ =====
    if (event.httpMethod === 'POST') {
      const d = JSON.parse(event.body);

      // Ensure header row exists
      const headerCheck = await sheets.spreadsheets.values.get({
        spreadsheetId: SHEET_ID,
        range: `${SHEET_NAME}!A1:Z1`,
      });
      if (!headerCheck.data.values || headerCheck.data.values.length === 0) {
        await sheets.spreadsheets.values.update({
          spreadsheetId: SHEET_ID,
          range: `${SHEET_NAME}!A1`,
          valueInputOption: 'RAW',
          requestBody: { values: [[
            'ID','Created At','SA No.','Date','Customer Code','Customer Name',
            'Job No.','Job Name','REV','Boiler Type','Brand','Capacity',
            'MFG Year','Fuel','Working Pressure','Scope','Engineer',
            'Photo Count','Photos (JSON)'
          ]] },
        });
      }

      // Compress photos: resize to max 800px wide (already done client-side)
      const photosJson = JSON.stringify(d.photos || []);

      await sheets.spreadsheets.values.append({
        spreadsheetId: SHEET_ID,
        range: `${SHEET_NAME}!A1`,
        valueInputOption: 'RAW',
        insertDataOption: 'INSERT_ROWS',
        requestBody: { values: [[
          d.id,
          d.createdAt,
          d.docNumber       || '',
          d.date            || '',
          d.customerCode    || '',
          d.customerName    || '',
          d.jobNumber       || '',
          d.jobName         || '',
          d.rev             || '00.',
          d.boilerType      || '-',
          d.brand           || '-',
          d.capacity        || '-',
          d.mfgYear         || '-',
          d.fuel            || '-',
          d.workingPressure || '-',
          d.scope           || '',
          d.engineer        || '',
          String(d.photos ? d.photos.length : 0),
          photosJson,
        ]] },
      });

      return { statusCode: 200, headers, body: JSON.stringify({ ok: true }) };
    }

    return { statusCode: 405, headers, body: JSON.stringify({ error: 'Method not allowed' }) };

  } catch (err) {
    console.error('delivery-proxy error:', err);
    return { statusCode: 500, headers, body: JSON.stringify({ error: err.message }) };
  }
};
