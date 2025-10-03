# Unscheduled Jobs - Impressive Batteries Map

A web application that displays job locations from a Google Spreadsheet on an interactive Google Maps interface.

## 🚀 Quick Start

### Prerequisites

1. **Google Cloud Account** - You need a Google Cloud project with:
   - Google Maps JavaScript API enabled
   - Google Sheets API enabled
   - An API key with access to both APIs

2. **Google Spreadsheet** - Your spreadsheet should be:
   - Publicly accessible (Share > Anyone with the link can view)
   - Formatted with columns containing latitude, longitude, and job information

### Step 1: Set Up Google Cloud

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the following APIs:
   - **Google Maps JavaScript API**: [Enable here](https://console.cloud.google.com/marketplace/product/google/maps-backend.googleapis.com)
   - **Google Sheets API**: [Enable here](https://console.cloud.google.com/marketplace/product/google/sheets.googleapis.com)
4. Create an API Key:
   - Go to **APIs & Services > Credentials**
   - Click **+ CREATE CREDENTIALS > API Key**
   - Copy the API key
   - (Optional but recommended) Click **Edit API Key** and restrict it:
     - **Application restrictions**: HTTP referrers (for production, add your Vercel domain)
     - **API restrictions**: Restrict to Google Maps JavaScript API and Google Sheets API

### Step 2: Prepare Your Google Spreadsheet

1. Make sure your spreadsheet is formatted correctly:
   ```
   Row 1 (Headers):
   Latitude | Longitude | Title | Description | Address | Contact | Notes
   
   Row 2+ (Data):
   37.7749  | -122.4194 | Job 1 | Install batteries | 123 Main St | John | Urgent
   ```

2. Make the spreadsheet publicly accessible:
   - Click **Share** button
   - Change to "Anyone with the link can view"
   - Copy the spreadsheet ID from the URL:
     ```
     https://docs.google.com/spreadsheets/d/SPREADSHEET_ID_HERE/edit
     ```

### Step 3: Configure the Application

1. Open `index.html`
2. Find the `CONFIG` section (around line 145)
3. Update the following values:

```javascript
const CONFIG = {
    // Your Google Maps API Key
    GOOGLE_MAPS_API_KEY: 'YOUR_ACTUAL_API_KEY',
    
    // Your Google Sheets ID from the URL
    GOOGLE_SHEETS_ID: 'YOUR_ACTUAL_SPREADSHEET_ID',
    
    // The name of your sheet/tab
    SHEET_NAME: 'Sheet1',
    
    // Column mapping (0 = Column A, 1 = Column B, etc.)
    COLUMNS: {
        recordId: 0,          // Column A - Record ID
        customerName: 1,      // Column B - Customer Name
        projectAddress: 2,    // Column C - Project Address
        latitude: 3,          // Column D - Latitude
        longitude: 4,         // Column E - Longitude
        systemSize: 5,        // Column F - System Size [kW]
        batterySize: 6,       // Column G - Battery Size [kWh]
        phases: 7,            // Column H - Phases
        batteryLocation: 8,   // Column I - Battery Location
        installationDate: 9   // Column J - Installation date
    }
};
```

**Important**: Adjust the `COLUMNS` mapping to match your spreadsheet structure. The numbers represent column indices (0 = A, 1 = B, 2 = C, etc.)

### Step 4: Test Locally

1. Open `index.html` in a web browser
2. You should see a map with markers for each job location
3. Click on markers to see job details

## 📦 Deploy to Vercel

### Option 1: Using Vercel CLI

1. Install Vercel CLI:
   ```bash
   npm install -g vercel
   ```

2. Deploy:
   ```bash
   vercel
   ```

3. Follow the prompts:
   - Set up and deploy? **Y**
   - Which scope? Choose your account
   - Link to existing project? **N**
   - Project name? Press enter (or choose a name)
   - Directory? Press enter (current directory)

4. Your site will be deployed! You'll get a URL like: `https://your-project.vercel.app`

### Option 2: Using Vercel Dashboard

1. Go to [vercel.com](https://vercel.com) and sign up/login
2. Click **Add New... > Project**
3. Import your Git repository (push your code to GitHub first), or
4. Upload this directory directly
5. Click **Deploy**

### After Deployment

If you restricted your API key in Google Cloud Console:
1. Go back to your API key settings
2. Add your Vercel domain to the HTTP referrer list:
   ```
   https://your-project.vercel.app/*
   https://*.vercel.app/*
   ```

## 📊 Spreadsheet Format

Your Google Spreadsheet should follow this format:

| Column | Name | Description | Required |
|--------|------|-------------|----------|
| A | Record ID | Unique identifier for the job | Optional |
| B | Customer Name | Customer's name | Yes |
| C | Project Address | Installation address | Yes |
| D | Latitude | Decimal latitude (-90 to 90) | Yes |
| E | Longitude | Decimal longitude (-180 to 180) | Yes |
| F | System Size [kW] | Solar system size in kW | Optional |
| G | Battery Size [kWh] | Battery capacity in kWh | Optional |
| H | Phases | Number of phases (1 or 3) | Optional |
| I | Battery Location | Where battery is installed | Optional |
| J | Installation Date | Scheduled installation date | Optional |

**Example Data:**
```
Record ID | Customer Name  | Project Address      | Latitude  | Longitude | System Size | Battery Size | Phases | Battery Location | Installation Date
001       | John Smith     | 123 Main St, Sydney  | -33.8688  | 151.2093  | 10          | 13.5         | 3      | Garage          | 2025-10-15
002       | Jane Doe       | 456 Oak Ave, Sydney  | -33.8567  | 151.2154  | 6.6         | 10           | 1      | External        | 2025-10-20
```

## 🛠️ Customization

### Change Map Style
Edit the `styles` array in the `initMap()` function (line 199) to customize the map appearance.

### Change Marker Icons
Add custom marker icons in the `google.maps.Marker` options:
```javascript
icon: {
    url: 'https://your-custom-icon.png',
    scaledSize: new google.maps.Size(40, 40)
}
```

### Modify Info Window Layout
Edit the `createInfoWindowContent()` function (line 327) to change what information is displayed.

## 🔧 Troubleshooting

### Map not loading?
- Check that your Google Maps API key is correct
- Verify that Google Maps JavaScript API is enabled in Google Cloud Console
- Check browser console for error messages

### No markers appearing?
- Verify your spreadsheet is publicly accessible
- Check that the spreadsheet ID is correct
- Ensure the sheet name matches your tab name
- Verify latitude and longitude values are valid numbers
- Check browser console for errors

### "Failed to fetch data" error?
- Verify Google Sheets API is enabled
- Check that the spreadsheet ID is correct
- Make sure the spreadsheet is shared publicly

## 📝 Notes

- The first row of your spreadsheet is treated as headers and will be skipped
- Invalid coordinates (non-numeric or 0,0) will be automatically skipped
- The map will automatically zoom to fit all markers
- All data is loaded client-side (no backend required)

## 🔐 Security Considerations

For production use:
1. Restrict your API key to specific domains in Google Cloud Console
2. Consider using environment variables for sensitive data
3. Monitor API usage in Google Cloud Console
4. Set up billing alerts to avoid unexpected charges

## 📄 License

This project is provided as-is for use with Impressive Batteries installation management.

