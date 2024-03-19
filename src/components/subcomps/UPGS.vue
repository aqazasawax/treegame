<script>
export const UPGRADES = {
  1.11: {
    cost: 10,
    baseCost: 10,
    costScaling: 4,
    desc: 'Strengthen your body - double point gain',
    purchased: false,
    purchaseAmount: 0,
  },
  1.21: {
    cost: 20,
    baseCost: 20,
    costScaling: 5,
    desc: 'Train your stamina - add 1 to base point gain',
    purchased: false,
    purchaseAmount: 0,
  },
  1.22: {
    cost: 1000,
    baseCost: 1000,
    desc: 'automatically gain points',
    purchased: false,
    single: true,
  },
  2.11: {
    cost: 2000,
    baseCost: 2000,
    desc: 'Learn the basics of swordsmanship - boost point gain based on total upgrades purchased',
    purchased: false,
    single: true,
  },
}
</script>