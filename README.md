module.exports = {
    name: 'stats',
    description: "User Stats.",
    execute(message, args){

      // Includes

      const fs = require('fs');
      const moment = require('moment')

      // Functions

      function formatNumber(num) {
          return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')
      }

      // Arguments

      let userData = JSON.parse(fs.readFileSync('Storage/userData.json', 'utf8'));

      // Variables

  		var sender = message.author;
  		var msg = message.content.toUpperCase();

      // Create User Profile if N/A

      if (!userData[sender.id]) userData[sender.id] = {}  							                		  // Creates JSON file for user
      if (!userData[sender.id].money) userData[sender.id].money = 0;                          // Creates user money
      if (!userData[sender.id].lastDaily) userData[sender.id].lastDaily = 'Not Collected';    // Creates user daily
      if (!userData[sender.id].diceWinnings) userData[sender.id].diceWinnings = 0;            // Creates dice winnings
      if (!userData[sender.id].duelWinnings) userData[sender.id].duelWinnings = 0;            // Creates duel diceWinnings
      if (!userData[sender.id].duelsWon) userData[sender.id].duelsWon = 0;                    // Creates duels won
      if (!userData[sender.id].duelsLost) userData[sender.id].duelsLost = 0;                  // Creates duels lost

      // Stats Flow

      message.channel.send({embed:{
        title:"RuneStake Stats",
        color:"0xF1C40F",
        fields:[
        {
          name:"User",
          value:message.author.username,
          inline:true
        },
        {
          name:"Current GP",
          value:formatNumber(userData[sender.id].money),
          inline:true
        },
        {
          name:"Dice Winnings",
          value:formatNumber(userData[sender.id].diceWinnings),
          inline:false
        },
        {
          name:"Duel Winnings",
          value:formatNumber(userData[sender.id].duelWinnings),
          inline:true
        },
        {
          name:"Duels Won",
          value:formatNumber(userData[sender.id].duelsWon),
          inline:true
        },
        {
          name:"Duels Lost",
          value:formatNumber(userData[sender.id].duelsLost),
          inline:true
        }

      ]
      }})

      // Write changes

      fs.writeFile('Storage/userData.json', JSON.stringify(userData), (err) => {			// Writes changes to created JSON file
          if (err) console.error(err);}
      )

    }
}
