## Plutus vs Helios 
A side-by-side code comparison of plutus and helios languages for implementing smart contracts

#### Constructor for Pending or Balanced Transactions

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```

data TxInfo = TxInfo
    { txInfoInputs      :: [TxInInfo] 
    
    , txInfoOutputs     :: [TxOut]
    , txInfoFee         :: Value 
    , txInfoMint        :: Value
    , txInfoDCert       :: [DCert]
    , txInfoWdrl        :: Map StakingCredential Integer
    , txInfoValidRange  :: POSIXTimeRange 
    , txInfoSignatories :: [PubKeyHash] 
    , txInfoRedeemers   :: Map ScriptPurpose Redeemer
    , txInfoData        :: Map DatumHash Datum
    , txInfoId          :: TxId
    }
```

</td>

<td>

```
Tx::new(
    inputs:      []TxInput,
    ref_inputs:  []TxInput,
    outputs:     []TxOutput,
    fee:         Value,
    minted:      Value,
    dcerts:      []DCert,
    withdrawals: Map[StakingCredential]Int,
    time_range:  TimeRange,
    signatories: []PubKeyHash,
    redeemers:   Map[ScriptPurpose]AnyType,
    datums:      Map[DatumHash]AnyType
) -> Tx
```

</td>
</tr>
</table>

<h4> Getters </h4>

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```

tx:: TxInfo

txInfoInputs      tx :: [TxInInfo] 

txInfoOutputs     tx :: [TxOut]
txInfoFee         tx :: Value 
txInfoMint        tx :: Value
txInfoDCert       tx :: [DCert]
txInfoWdrl        tx :: Map StakingCredential Integer
txInfoValidRange  tx :: POSIXTimeRange 
txInfoSignatories tx :: [PubKeyHash] 
txInfoRedeemers   tx :: Map ScriptPurpose Redeemer
txInfoData        tx :: Map DatumHash Datum
txInfoId          tx :: TxId

```

</td>

<td>

```
tx: Tx = Tx::new(..);

tx.inputs      -> []TxInput
tx.ref_inputs  -> []TxInput
tx.outputs     -> []TxOutput
tx.fee         -> Value
tx.minted      -> Value,
tx.dcerts      -> []DCert,
tx.withdrawals -> Map[StakingCredential]Int,
tx.time_range  -> TimeRange,
tx.signatories -> []PubKeyHash,
tx.redeemers   -> Map[ScriptPurpose]AnyType,
tx.datums      -> Map[DatumHash]AnyType

```

</td>
</tr>
</table>
