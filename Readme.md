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

<h4> Validator Methods </h4>

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```
tx:: TxInfo
-- Signature
txSignedBy :: TxInfo -> PubKeyHash -> Bool
eg. txSignedBy tx mPubKeyHash

findDatumHash :: Datum -> TxInfo -> Maybe DatumHash
--
--
valueLockedBy :: TxInfo -> ValidatorHash -> Value
scriptOutputsAt :: ValidatorHash -> TxInfo -> [(DatumHash, Value)]
--
valuePaidTo :: TxInfo -> PubKeyHash -> Value
--
valueLockedBy :: TxInfo -> ValidatorHash -> Value
--
findDatum :: DatumHash -> TxInfo -> Maybe Datum
findTxInByTxOutRef :: TxOutRef -> TxInfo -> Maybe TxInInfo
pubKeyOutputsAt :: PubKeyHash -> TxInfo -> [Value]
valueSpent :: TxInfo -> Value
valueProduced :: TxInfo -> Value
spendsOutput :: TxInfo -> TxId -> Integer -> Bool

```

</td>

<td>

```
tx: Tx = Tx::new(..)
// Signature
is_signed_by(pubkeyhash: PubKeyHash) -> Bool
eg. tx.is_signed_by(mPubKeyHash)

tx.find_datum_hash(data: AnyType) -> ByteArray
tx.outputs_sent_to(pkh: PubKeyHash) -> []TxOutput
tx.outputs_sent_to_datum(pkh: PubKeyHash, datum: AnyType, is_inline: Bool) -> []TxOutput
tx.value_locked_by(script_hash: ValidatorHash) -> Value
tx.outputs_locked_by(script_hash: ValidatorHash) -> []TxOutput
tx.outputs_locked_by_datum(script_hash: ValidatorHash, datum: AnyType, is_inline: Bool) -> []TxOutput
tx.value_sent_to(addr: PubKeyHash) -> Value
tx.value_sent_to_datum(addr: PubKeyHash, datum: AnyType, is_inline: Bool) -> Value
tx.value_locked_by(script_hash: ValidatorHash) -> Value
tx.value_locked_by_datum(script_hash: ValidatorHash, datum: AnyType, is_inline: Bool) -> Value
//
//
//
//
//
//

```

</td>
</tr>
</table>

<h4> Transaction Output Constructor </h4>

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```
data TxOut = TxOut {
    txOutAddress   :: Address,
    txOutValue     :: Value,
    txOutDatumHash :: Maybe DatumHash
    }
    
    
```

</td>

<td>

```
TxOutput::new(
    address: Address,
    value:   Value,
    datum:   OutputDatum
) -> TxOutput

    
```

</td>
</tr>
</table>

<h4> Getter Methods </h4>

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```
tx_output TxOut

txOutAddress   tx_output :: Address
txOutValue     tx_output :: Value
txOutDatumHash tx_output :: Maybe DatumHash
    
```

</td>

<td>

```
tx_output: TxOutput = TxOutput::new(..)
    
tx_output.address -> Address
tx_output.value   -> Value
tx_output.datum   -> OutputDatum
    
```

</td>
</tr>
</table>

<h2> Script Context </h2>

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```
-- The context that the currently-executing script can access.
data ScriptContext = ScriptContext
    { scriptContextTxInfo  :: TxInfo 
    , scriptContextPurpose :: ScriptPurpose 
    }
    
```

</td>

<td>

```
// scriptContextPurpose = new_minting || new_rewarding         || new_spending
ScriptContext::[scriptContextPurpose](
    tx:  Tx,
    mph: MintingPolicyHash            || sc: StakingCredential || output_id: TxOutputId
) -> ScriptContext
    
```

</td>
</tr>
</table>


<h4> Script Context Methods</h4>

<table>
<tr>
<td> Plutus (Haskell) </td> <td> Helios </td>
</tr>
<tr>
<td>

```
findOwnInput :: ScriptContext -> Maybe TxInInfo
findContinuingOutputs :: ScriptContext -> [Integer]
getContinuingOutputs :: ScriptContext -> [TxOut]
ownCurrencySymbol :: ScriptContext -> CurrencySymbol
    
```

</td>

<td>

```
ctx.get_current_input() -> TxInput
//
ctx.get_cont_outputs() -> []TxOutput
//
    
```

</td>
</tr>
</table>

