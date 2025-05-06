# git-clone-https-github.com-your-username-DukeXReserveBankconst express = require('express');
const app = express();
const stripe = require('stripe')('sk_live_
app.post('/create-wallet-pass', async (req, res) => {
  const { type } = req.body;

  try {
    const cardholder = await stripe.issuing.cardholders.create({
      name: 'Estdevon Perkins',
      type: 'individual',
      email: 'estdevon@example.com',
      billing: {
        address: {
          line1: '123 Main St',
          city: 'Trenton',
          state: 'NJ',
          postal_code: '08608',
          country: 'US'
        }
      }
    });

    const card = await stripe.issuing.cards.create({
      cardholder: cardholder.id,
      currency: 'usd',
      type: 'virtual',
    });

    const walletPass = await stripe.issuing.cards.createWalletPass(card.id);

    res.json({
      cardId: card.id,
      clientSecret: walletPass.client_secret
    });
  } catch (err) {
    console.error(err);
    res.status(500).json({ error: err.message });
  }
});

app.listen(4242, () => console.log('Server running on port 4242'));
