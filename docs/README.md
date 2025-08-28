# Monopoly Deal Score Tracker

A simple, responsive web application for tracking Monopoly Deal game scores across gaming sessions. Built for weekly Xbox gaming sessions with automatic score updates from Google Sheets.

## Features

- **Real-time Score Tracking**: Automatically fetches latest scores from Google Sheets
- **Season Statistics**: Track total games played and last session summary
- **Leader Detection**: Automatically identifies current leader(s) with tie detection
- **Player Avatars**: Xbox Live profile images for each player
- **Mobile Responsive**: Optimized for all device sizes
- **Session Analytics**: Mixpanel integration for usage tracking

## Architecture

```
User Input → Google Form → Google Sheets → Published CSV → Web App
```

### Data Flow
1. Game results entered via Google Form
2. Form submissions stored in Google Sheets
3. Sheet published as CSV feed
4. Web app fetches and processes CSV data
5. Displays formatted scoreboard with statistics

## Technical Stack

- **Frontend**: Vanilla HTML/CSS/JavaScript
- **Styling**: CSS Custom Properties, Google Fonts (Poppins, Anton)
- **Data Source**: Google Sheets (published as CSV)
- **Hosting**: GitHub Pages
- **Domain**: Custom domain via Cloudflare DNS
- **Analytics**: Mixpanel

## Configuration

### Player Data
Players are configured in the `playersData` object:
```javascript
const playersData = {
    "PlayerName": { 
        wins: 0, 
        losses: 0, 
        avatar: "xbox_live_avatar_url"
    }
};
```

### Google Sheets Integration
The app connects to Google Sheets via a published CSV.

## Features Deep Dive

### Score Calculation
- **Wins**: Direct count from Google Sheets entries
- **Losses**: Total games minus individual wins
- **Leader**: Player(s) with most wins (handles ties)

### Session Statistics
- **Games This Season**: Total entries in sheet
- **Last Session**: Games played on most recent past date (excludes today)
- **Date Display**: DD/MM/YYYY format

## Troubleshooting

### Common Issues

#### **Scores Not Loading**
**Symptoms**: "Loading scores..." message persists
**Possible Causes**:
- Google Sheets not published correctly
- CSV URL changed or invalid
- Network connectivity issues
- CORS restrictions

**Debug Steps**:
1. Check browser console for fetch errors
2. Verify CSV URL returns data when accessed directly
3. Confirm Google Sheet is published (not just shared)
4. Check if sheet structure matches expected format

#### **Player Not Appearing**
**Symptoms**: Player name in sheet but not showing on scoreboard
**Cause**: Name mismatch between sheet and `playersData` object
**Fix**: Ensure exact case-sensitive match between sheet entries and player configuration

#### **Incorrect Win/Loss Counts**
**Symptoms**: Numbers don't match expected results
**Debug Steps**:
1. Check raw CSV data for duplicate entries
2. Verify date parsing (DD/MM/YYYY format expected)
3. Look for empty rows or malformed entries
4. Check browser console for parsing warnings

#### **Avatars Not Loading**
**Symptoms**: Broken image icons instead of player avatars
**Cause**: Xbox Live avatar URLs expired or changed
**Fix**: Update avatar URLs in `playersData` configuration

### Expected Data Format

Google Sheets should have columns:
```
Timestamp | Who won?
DD/MM/YYYY HH:MM:SS | PlayerName
```

### Browser Support
- Modern browsers
- Chrome, Firefox, Safari, Edge
- Mobile browsers on iOS/Android

## Analytics

The app tracks usage via Mixpanel:
- Page views
- Score refreshes  
- Sound effect usage
- External link clicks

## Development

### Local Development
1. Clone repository
2. Serve files via local web server (required for CORS)
3. Update `sheetUrl` if testing with different data source

### CI/CD Pipeline
The site uses GitHub Actions for automated deployment:

1. **Trigger**: Push commits to main branch (`dynamic` trigger)
2. **Build Step**: GitHub Actions builds static site (~25s)
3. **Status Report**: Build status validation (~5s)  
4. **Deploy Step**: Deployment to GitHub Pages (~10s)
5. **Artifact**: Creates `github-pages` artifact (1.15 MB)
6. **Live**: Site updates in ~45 seconds total

**Workflow**: `pages build and deployment` (GitHub's default Pages Action)
**DNS**: Cloudflare manages custom domain routing
**SSL**: GitHub Pages provides SSL certificate and HTTPS enforcement

### Deployment Process
- Automatic deployment via GitHub Pages
- Triggered on every commit to main branch
- Custom domain configured through Cloudflare
- SSL certificate managed by Github Pages
- Typical deployment time: 1-5 minutes

## External Integration

The application integrates with:
- **Google Forms**: For score submission
- **Google Sheets**: For data storage and CSV publishing
- **Mixpanel**: For usage analytics

*Access to score submission and data management is controlled through the live application interface.*

## License

Open source project. Feel free to fork and adapt for your own gaming groups.

**Note for forkers:**
- Replace Mixpanel token with your own analytics setup
- Update Google Sheets URL to point to your own data source  
- Replace Xbox Live avatar URLs with your own player images
- Xbox Live avatars may have usage restrictions outside personal use

---
