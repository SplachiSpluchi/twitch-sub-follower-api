const express = require('express');
const fetch = require('node-fetch');

const app = express();
const port = process.env.PORT || 3000;

// Twitch credentials
const clientID = process.env.CLIENT_ID;
const oauthToken = process.env.OAUTH_TOKEN;
const channelID = process.env.CHANNEL_ID;

// Helper function to fetch subscriber count
async function getTwitchData() {
    const url = `https://api.twitch.tv/helix/subscriptions?broadcaster_id=${channelID}`;
    const headers = {
        'Client-ID': clientID,
        'Authorization': `Bearer ${oauthToken}`
    };
    
    const response = await fetch(url, { method: 'GET', headers });
    const data = await response.json();
    
    // Fetch follower count as well
    const followerUrl = `https://api.twitch.tv/helix/users/follows?to_id=${channelID}`;
    const followerResponse = await fetch(followerUrl, { method: 'GET', headers });
    const followerData = await followerResponse.json();
    
    return {
        subs: data.data.length,  // Number of subscribers
        followers: followerData.total // Number of followers
    };
}

// API endpoint to fetch stats
app.get('/twitch-stats', async (req, res) => {
    try {
        const stats = await getTwitchData();
        res.json(stats);
    } catch (error) {
        res.status(500).json({ error: 'Failed to fetch Twitch stats' });
    }
});

// Start server
app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
