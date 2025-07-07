# Solana-Staking-Platform---Complete-Requirements
Solana Staking Platform - Complete Requirements
# Solana Staking Platform - Complete Requirements
*Last Updated: January 2025*
*Version: 1.1 - Added Special Wallets, Airdrops, and Enhanced Tracking*

## Platform Overview
A Solana validator staking platform with token-gated rewards, transparent on-chain operations, and comprehensive admin controls.

### Key Features:
- **Dual Reward System**: 70% rewards for token holders, 20% for non-holders
- **Special Wallet Overrides**: Custom rates (0-100%) for employees, partners, VIPs
- **Proportional Distribution**: Rewards based on % of total stake
- **Batch Processing**: Platform pays gas for regular distributions
- **Full Admin Control**: All parameters adjustable via dashboard
- **Transparent Treasury**: 5-wallet split for platform funds
- **Community Features**: STEM donations and SOL giveaways
- **Airdrop System**: Track metrics and execute token airdrops
- **Rewards Accumulation Display**: Progress bar showing pending rewards

## User Flow
1. **Connect Wallet** - Any Solana compatible wallet
2. **First-Time Setup**
   - Enter unique username (checked against existing users on-chain)
   - Sign transaction to create user profile
   - Pay one-time account creation fee
3. **Returning Users**
   - Connect wallet → Sign message to authenticate → Access dashboard
4. **Staking Process**
   - Check minimum stake amount
   - Transfer SOL to platform vault
   - Update user's staked balance
   - Start earning rewards next epoch
5. **Claiming Rewards**
   - Wait for batch distribution (platform pays gas)
   - OR claim early (user pays gas)
   - Rewards based on stake % and token holder status

## Airdrop System

### Staker Metrics Tracking
**On-chain metrics tracked per staker:**
1. **Staking Loyalty Score**: Continuous days staked
2. **Total Volume**: Lifetime SOL staked amount
3. **Token Hold Duration**: Days holding 200k+ tokens
4. **Participation Score**: Number of distributions participated in
5. **First Stake Date**: When they joined the platform
6. **Last Active Epoch**: Most recent activity

### Admin Airdrop Dashboard
**View & Filter Stakers:**
- Sortable table with all staker metrics
- Filter by:
  - Minimum stake amount
  - Minimum token balance
  - Staking duration
  - Active in last X days
  - Special wallet status

**Execute Airdrops:**
1. Select recipients (individual checkboxes or criteria-based)
2. Set airdrop amount (per wallet or total to split)
3. Choose token to airdrop
4. Preview total cost and recipient count
5. Double confirmation before execution
6. On-chain execution with audit trail

### Integration with Rewards
- Airdrop eligibility can consider reward tier
- Special wallets can have airdrop multipliers
- Automatic airdrops based on milestones

## Special Wallet System

### Overview
Allows platform to give custom reward rates to specific wallets (employees, partners, advertisers, VIPs)

### Features
1. **Add Special Wallets**
   - Input wallet address
   - Set custom reward rate (0-100%)
   - Toggle token requirement ON/OFF
   - Add notes/labels for identification

2. **Use Cases**
   - Employee: 95% rewards, no token requirement
   - Partner: 80% rewards, no token requirement
   - VIP Staker: 100% rewards, token requirement ON
   - Test Wallet: 0% rewards for testing

3. **Management Interface**
   - Table view of all special wallets
   - Edit rates inline
   - Remove wallets
   - Bulk import/export
   - Activity tracking per wallet

## Edge Cases & Rules
1. **Token Balance Changes**
   - Checked at distribution time, not stake time
   - If tokens transferred out, user gets 20% rate at next distribution
   - Must hold tokens in same wallet as staking wallet
2. **Unstaking**
   - Can unstake anytime (no cooldown period)
   - Pending rewards distributed first
   - If early unstake, user pays gas for reward claim
3. **Platform Paused**
   - No new stakes accepted
   - Existing stakers can still unstake
   - Rewards continue accumulating

## Core Smart Contract Features

### 1. Staking Mechanics
- **Deposit SOL**: 
  - Transfer from user wallet to platform vault
  - Create/update staker account with amount and timestamp
  - Check if user meets minimum stake requirement
- **Withdraw SOL**:
  - Transfer from platform vault back to user
  - Update staker account balances
  - Calculate and distribute any pending rewards first
- **Auto-compound Option**: 
  - User toggle for automatic reward reinvestment
  - Compounds at distribution time, not continuously

### 2. Token-Gated Rewards System
- **Token Requirement**: Fixed amount of platform token (starting at 200,000 tokens, admin adjustable)
- **Standard Reward Rates**:
  - Token holders: 70% (scaling to 95% over time)
  - Non-token holders: 20%
- **Special Wallet Override System**:
  - Admin can set custom rates (0-100%) per wallet
  - Toggle token requirement ON/OFF per wallet
  - For employees, partners, advertisers, VIPs
- **Token Check**: At distribution time, must be in staking wallet (unless overridden)

### 3. Reward Distribution Mechanics
**How Rewards Are Calculated:**
- Each staker earns proportional to their stake amount
- Example: If total staked = 1000 SOL and you staked 100 SOL, you own 10% of the pool
- If validator earns 10 SOL in rewards for the epoch:
  - Your share = 1 SOL (10% of 10 SOL)
  - With token (70% rate): You receive 0.7 SOL
  - Without token (20% rate): You receive 0.2 SOL
  - Platform keeps the remainder (0.3 SOL or 0.8 SOL)

**Special Wallet Overrides:**
- Admin-set wallets use custom rates (0-100%)
- Override applies before standard rate calculation
- Example: Employee wallet at 95% rate gets 0.95 SOL instead of 0.7 SOL

**Complete Example:**
```
Validator earns: 100 SOL total for epoch
Platform receives: 100 SOL

Standard token holder (1% of total stake):
- Share of rewards: 1 SOL
- Receives: 0.7 SOL (70% rate)
- Platform keeps: 0.3 SOL

Special wallet (1% of total stake, 95% custom rate):
- Share of rewards: 1 SOL
- Receives: 0.95 SOL (95% rate)
- Platform keeps: 0.05 SOL
```

**Distribution Process:**
- Rewards accumulate in platform pool between distributions
- Real-time pending rewards shown (calculated off-chain for display)
- Pending rewards displayed as progress bar in user dashboard
- Every X epochs: Batch distribution to all stakers
- Platform pays all gas fees for batch distribution
- Users can claim early but must pay their own gas fees

**Reward Rate Priority Order:**
1. Check if wallet is in special wallet list → Use custom rate
2. Check if wallet holds 200k tokens → Use token holder rate (70%)
3. Otherwise → Use non-holder rate (20%)

### 4. Validator Integration
**Suggested Approach:**
- Create a platform stake account delegated to chosen validator
- Use Solana's stake pool program or custom implementation
- Validator rewards flow to platform stake account
- Platform claims rewards and distributes to users

### 4. Batch Distribution System
- **Frequency**: Every X epochs (admin configurable, suggested 3-4)
- **Gas Fees**: Platform pays for batch distributions
- **Early Claims**: Users can claim early but pay own gas
- **Real-time Display**: Show pending rewards between distributions

### 5. Treasury Management
Platform's share (30% initially → 5% eventually) splits into:
- **Community Staking Wallet**: 50%
- **Token Buyback & Burn**: 25%
- **Operations & Growth**: 20%
- **STEM Donation**: 2%
- **SOL Giveaway**: 3%

### 6. Donation Features
- **STEM Wallet Goal**: 
  - Displayed as "User Level/Tier" progress
  - Admin sets goal amount
  - Users can donate directly
- **Giveaway Wallet Goal**:
  - Displayed as "Rewards Goal"
  - Admin sets goal amount

## Admin Dashboard Requirements

### 1. Authentication
- Multi-sig wallet (Squads or similar)
- Secure login with wallet signature

### 2. Editable Parameters
**Staking Settings**
- [ ] Minimum stake amount
- [ ] Maximum stake amount
- [ ] Pause/unpause platform
- [ ] Validator selection

**Reward Settings**
- [ ] Token holder reward % (70-95%)
- [ ] Non-holder reward % (20%)
- [ ] Required token amount (starting at 200,000)
- [ ] Distribution frequency (epochs)

**Treasury Wallets**
- [ ] Community wallet address
- [ ] Buyback wallet address
- [ ] Operations wallet address
- [ ] STEM wallet address
- [ ] Giveaway wallet address

**Goal Settings**
- [ ] STEM donation goal amount
- [ ] Giveaway goal amount

**Special Wallet Management**
- [ ] Add/remove wallet addresses
- [ ] Set custom reward rate per wallet (0-100%)
- [ ] Toggle token requirement per wallet
- [ ] Bulk import wallet list
- [ ] Export special wallet settings

**Airdrop Management**
- [ ] View all stakers with metrics
- [ ] Filter by criteria (stake amount, duration, token balance)
- [ ] Select recipients for airdrops
- [ ] Set airdrop amounts
- [ ] Execute token airdrops
- [ ] View airdrop history

### 3. Change Confirmation Flow
1. Admin makes changes in dashboard
2. Click "Save" button
3. **First Modal**: "Are you sure you want to continue?" [Cancel] [Continue]
4. **Second Modal (Red)**: "This will update on-chain settings. Confirm?" [Cancel] [Confirm]
5. Multi-sig wallet prompts for signature
6. Transaction executes on-chain
7. Dashboard refreshes with new values
8. Audit log entry created

### 4. Admin Dashboard UI Elements
- **Toggle Switches**: For boolean values (paused, auto-compound defaults)
- **Input Fields**: For amounts and percentages
- **Wallet Address Fields**: For treasury wallets
- **Sliders**: For percentage adjustments with visual feedback
- **Save Button**: Triggers confirmation flow
- **Reset Button**: Reverts to current on-chain values
- **Activity Log**: Shows recent admin actions

## User Dashboard Display

### 1. Staking Information
- Current staked amount (SOL & USD)
- Pending rewards with accumulation progress bar
- Total earned lifetime
- APY display
- Next distribution countdown
- Special wallet status indicator (if applicable)

### 2. Progress Indicators
- **Staking Progress**: % of personal staking goal
- **STEM Goal Progress**: Platform progress to STEM donation goal
- **Giveaway Goal Progress**: Platform progress to giveaway goal
- **Epoch Progress**: Current epoch completion %

### 3. Charts & Analytics
- Auto-compound growth
- SOL price chart
- Platform token price chart
- Validator performance metrics

### 4. Transaction History
- Stake/unstake events
- Reward distributions
- Donation transactions

### 5. Additional UI Features (from dashboard images)
- **Notification Bell**: Alert users to distributions, goal achievements
- **User Profile Photo**: Integration with wallet avatars or custom uploads
- **Calendar View**: Visual distribution schedule with highlighted dates
- **Performance Metrics**: Validator uptime, APY, commission displays
- **Theme Toggle**: Dark/light mode preference stored locally

## Current Implementation Status

### Issues Found in Existing Code:
1. **No Actual Token Transfers** - Current handlers only update balances in state
2. **Missing state.rs** - Account structures not defined
3. **Incomplete Handlers** - Many functions have "// Implement your business logic here..."
4. **No Token-Gating Logic** - No checks for custom token holdings
5. **JavaScript Client Mismatch** - Shows different structure than Rust code expects

### Required Implementations:
- Add SPL token transfer instructions
- Implement vault system for holding staked SOL
- Add token balance checking for reward rates
- Create proper reward calculation logic
- Implement batch distribution mechanism

## Technical Architecture

### 1. Smart Contract Accounts (PDAs)

**PlatformConfig**
```rust
- authority: Pubkey
- staking_token_mint: Pubkey
- reward_token_mint: Pubkey
- min_stake_amount: u64
- max_stake_amount: u64
- token_holder_rate: u8 (70-95)
- non_holder_rate: u8 (20)
- required_token_amount: u64 (200,000)
- distribution_frequency: u8 (epochs)
- total_staked: u64
- is_paused: bool
- stem_goal: u64
- giveaway_goal: u64
```

**StakerInfo** (per user)
```rust
- owner: Pubkey
- staked_amount: u64
- pending_rewards: u64
- last_claim_epoch: u64
- auto_compound: bool
- total_earned: u64
- stake_timestamp: i64
- username: String
- first_stake_timestamp: i64
- continuous_stake_days: u32
- lifetime_staked: u64
- distributions_participated: u32
- token_hold_start: i64
- last_active_epoch: u64
```

**SpecialWallet** (per special wallet)
```rust
- wallet: Pubkey
- custom_rate: u8 (0-100)
- require_token: bool
- label: String
- added_by: Pubkey
- added_at: i64
- is_active: bool
```

**TreasuryConfig**
```rust
- community_wallet: Pubkey
- buyback_wallet: Pubkey
- operations_wallet: Pubkey
- stem_wallet: Pubkey
- giveaway_wallet: Pubkey
- community_percentage: u8 (50)
- buyback_percentage: u8 (25)
- operations_percentage: u8 (20)
- stem_percentage: u8 (2)
- giveaway_percentage: u8 (3)
```

**RewardPool**
```rust
- total_rewards: u64
- last_distribution_epoch: u64
- accumulated_rewards: u64
```

**UserProfile**
```rust
- wallet: Pubkey
- username: String
- created_at: i64
```

### 2. Program Instructions

**User Instructions:**
- `create_user_profile(username: String)`
  - Creates unique on-chain profile
  - Links wallet to username
  
- `stake(amount: u64)`
  - Transfers SOL from user to vault
  - Updates staker_info account
  - Validates minimum stake amount
  
- `unstake(amount: u64)`
  - Calculates pending rewards first
  - Transfers SOL from vault to user
  - Updates balances
  
- `claim_rewards()`
  - User pays gas
  - Checks token balance for rate
  - Transfers reward SOL to user
  
- `toggle_auto_compound()`
  - Flips boolean in staker account
  
- `donate_to_stem(amount: u64)`
  - Direct transfer to STEM wallet
  - Updates goal progress

**Admin Instructions:**
- `initialize_platform(config: PlatformConfig)`
  - One-time setup
  - Sets initial parameters
  
- `update_reward_rates(token_holder_rate: u8, non_holder_rate: u8)`
  - Requires multi-sig
  - Updates both rates
  
- `update_token_requirement(amount: u64)`
  - Changes required token amount
  
- `update_distribution_frequency(epochs: u8)`
  - Changes batch timing
  
- `update_treasury_wallets(wallets: TreasuryConfig)`
  - Updates all 5 wallet addresses
  
- `set_goals(stem_goal: u64, giveaway_goal: u64)`
  - Updates display goals
  
- `distribute_rewards()`
  - Batch distribution to all stakers
  - Platform pays gas
  - Calculates each user's share
  - Applies custom rates for special wallets
  
- `emergency_pause(paused: bool)`
  - Halts new stakes
  - Allows unstaking

- `add_special_wallet(wallet: Pubkey, rate: u8, require_token: bool, label: String)`
  - Adds wallet to special list
  - Sets custom parameters
  
- `update_special_wallet(wallet: Pubkey, rate: u8, require_token: bool, active: bool)`
  - Modifies existing special wallet
  
- `remove_special_wallet(wallet: Pubkey)`
  - Removes from special list
  
- `execute_airdrop(recipients: Vec<Pubkey>, amount_per_wallet: u64, token_mint: Pubkey)`
  - Distributes tokens to selected wallets
  - Creates audit log entry

### 3. Security Considerations
- All changes require multi-sig
- Audit trail for admin actions
- Rate limiting on critical operations
- Slashing protection for validator selection

## Reward Calculation Examples

### Scenario 1: Token Holder
- User stakes: 100 SOL
- Total platform stake: 1000 SOL
- User's share: 10%
- Validator earns: 50 SOL in epoch
- User's portion: 5 SOL
- With 70% rate: User gets 3.5 SOL
- Platform keeps: 1.5 SOL

### Scenario 2: Non-Token Holder  
- Same stake (100 SOL, 10% of pool)
- Same validator rewards (5 SOL share)
- With 20% rate: User gets 1 SOL
- Platform keeps: 4 SOL

### Scenario 3: Special Wallet (Employee)
- Same stake (100 SOL, 10% of pool)
- Same validator rewards (5 SOL share)
- Custom 95% rate: User gets 4.75 SOL
- Platform keeps: 0.25 SOL
- Token requirement: OFF (no need to hold tokens)

### Platform Distribution (from kept rewards)
If platform keeps 10 SOL total in an epoch:
- Community Staking: 5 SOL (50%)
- Buyback & Burn: 2.5 SOL (25%)
- Operations: 2 SOL (20%)
- STEM: 0.2 SOL (2%)
- Giveaway: 0.3 SOL (3%)

## Formulas
```
user_share = (user_stake / total_stake) * validator_rewards

// Check special wallet first
if is_special_wallet(user) {
    reward_rate = special_wallet.custom_rate / 100
} else if has_tokens {
    reward_rate = token_holder_rate / 100
} else {
    reward_rate = non_holder_rate / 100
}

user_rewards = user_share * reward_rate
platform_share = user_share * (1 - reward_rate)
```

1. **Token Amount**: Starting with 200,000 tokens
   - Admin adjustable through dashboard
   - Can be modified based on token price/market conditions

2. **Distribution Frequency**: How many epochs between distributions?
   - Suggestion: 3-4 epochs (6-8 days)

3. **Username System**: On-chain or off-chain storage?
   - Suggestion: On-chain for full decentralization

4. **Price Display**: Which oracle for SOL/USD conversion?
   - Options: Pyth, Switchboard, or Chainlink

5. **Validator Selection**: Single validator or multiple?
   - Suggestion: Start with one, expand later

## Next Steps
- [ ] Create missing state.rs file with all account structures
- [ ] Implement actual token transfer logic in handlers
- [ ] Add token balance checking for reward rates
- [ ] Choose multi-sig solution (Squads recommended)
- [ ] Design admin UI mockups
- [ ] Implement validator stake account creation
- [ ] Add batch distribution mechanism
- [ ] Create off-chain service for real-time reward calculations
- [ ] Set up program upgrade authority with multi-sig
- [ ] Implement comprehensive testing suite

## React/Web App Integration

### Frontend Requirements
1. **Wallet Integration**
   - Support all major Solana wallets (Phantom, Solflare, etc.)
   - Web3 wallet adapter
   - Transaction signing for all operations

2. **Real-Time Data Display**
   - Fetch on-chain data via RPC
   - Calculate pending rewards client-side between distributions
   - WebSocket for live updates
   - Price feeds for USD conversions

3. **User Dashboard Components**
   - Staking widget with amount input
   - Rewards tracker with claim button
   - Auto-compound toggle
   - Transaction history table
   - Progress charts (Chart.js or similar)
   - Epoch countdown timer

4. **Admin Dashboard Components**
   - Secure route with wallet authentication
   - Form inputs for all parameters
   - Preview changes before submission
   - Transaction status monitoring
   - Audit log display

### Data Flow
```
User Action → React App → Wallet Signing → Solana Program → Update State
     ↑                                                            ↓
     ←────────────── RPC Query for Updated Data ←───────────────
```

## Modified Token Staking Template Structure

### Starting Template Files
```
src/
├── instructions/
│   ├── create_config.rs    → Modify for platform initialization
│   ├── stake.rs           → Change to stake SOL, check token balance
│   ├── unstake.rs         → Add reward calculation
│   └── unlock.rs          → Repurpose for claim_rewards
└── state/
    ├── config.rs          → Expand for PlatformConfig
    ├── stake_position.rs  → Modify for StakerInfo
    └── user.rs            → Add username, metrics
```

### Files to Add
```
src/
├── instructions/
│   ├── create_user_profile.rs
│   ├── distribute_rewards.rs
│   ├── update_config.rs
│   ├── donate_to_stem.rs
│   ├── claim_validator_rewards.rs
│   ├── add_special_wallet.rs
│   ├── update_special_wallet.rs
│   └── execute_airdrop.rs
└── state/
    ├── treasury_config.rs
    ├── reward_pool.rs
    ├── validator_account.rs
    ├── special_wallet.rs
    └── airdrop_record.rs
```

### Key Modifications Needed
1. **create_config.rs**: Add validator setup, treasury wallets, initial rates
2. **stake.rs**: 
   - Accept SOL instead of SPL tokens
   - Check for 200k token balance
   - Check special wallet list
   - Create stake account for validator
3. **distribute_rewards.rs**: 
   - Calculate proportional shares
   - Apply custom rates for special wallets
   - Execute batch transfers

## Development Priorities
1. **Fix Core Staking**: Add actual SOL transfers
2. **Add Token Gating**: Implement balance checks
3. **Reward Distribution**: Build batch system
4. **Admin Dashboard**: Create secure management interface
5. **User Interface**: Connect React app to on-chain data

---
*This document will be updated as implementation progresses and decisions are finalized.*
