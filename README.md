# Data-Cleaning-SQL-project
Select *
From PorfolioProject..NashvilleHousing


-- Standardize Date Format

select SaleDateConverted,
CONVERT(date, SaleDate) from PorfolioProject..NashvilleHousing


update NashvilleHousing
set SaleDate = CONVERT(date, SaleDate)


Alter table NashvilleHousing
add SaleDateConverted Date;

update NashvilleHousing
set SaleDateConverted = CONVERT(date, SaleDate)


-- Populate Property Address data

select *
from PorfolioProject.dbo.NashvilleHousing
--Where PropertyAddress is null
order by ParcelID


select a.ParcelID, b.ParcelID, a.PropertyAddress, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)
from PorfolioProject.dbo.NashvilleHousing a
join PorfolioProject.dbo.NashvilleHousing b
	on a.ParcelID = b.ParcelID
	And a.[UniqueID ] <> b.[UniqueID ]
Where a.PropertyAddress is null

select PropertyAddress 
from PorfolioProject.dbo.NashvilleHousing
Where PropertyAddress is null

update a 
set propertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
from PorfolioProject.dbo.NashvilleHousing a
join PorfolioProject.dbo.NashvilleHousing b
	on a.ParcelID = b.ParcelID
	And a.[UniqueID ] <> b.[UniqueID ]
Where a.PropertyAddress is null


-- Breaking out Address into Individual Columns (Address, City, State)


Select PropertyAddress
From PorfolioProject.dbo.NashvilleHousing
--Where PropertyAddress is null
--order by ParcelID

select 
SUBSTRING(PropertyAddress, 1, CHARINDEX(',' , PropertyAddress) -1) as Address,
SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress)) as Address
from PorfolioProject.dbo.NashvilleHousing


Alter table NashvilleHousing
add PropertySplityAddress Nvarchar(225);

update NashvilleHousing
set PropertySplityAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',' , PropertyAddress) -1)


Alter table NashvilleHousing
add PropertySplitCity Nvarchar(225);

update NashvilleHousing
set PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress))

select * 
from PorfolioProject.dbo.NashvilleHousing


Select OwnerAddress
 from PorfolioProject.dbo.NashvilleHousing


 Select
PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)
,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)
,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)
From PorfolioProject.dbo.NashvilleHousing



Alter table NashvilleHousing
add OwnerSplitAddress Nvarchar(225);

update NashvilleHousing
set  OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)


Alter table NashvilleHousing
add OwnerSplitCity Nvarchar(225);

update NashvilleHousing
set OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)


ALTER TABLE NashvilleHousing
Add OwnerSplitState Nvarchar(255);

Update NashvilleHousing
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)


select * 
from PorfolioProject.dbo.NashvilleHousing



-- Change Y and N to Yes and No in "Sold as Vacant" field


Select Distinct(SoldAsVacant), Count(SoldAsVacant)
from PorfolioProject.dbo.NashvilleHousing
Group by SoldAsVacant
order by 2



Select SoldAsVacant, 
	CASE When SoldAsVacant = 'Y' THEN 'Yes'
	   When SoldAsVacant = 'N' THEN 'No'
	   ELSE SoldAsVacant
	   END
from PorfolioProject.dbo.NashvilleHousing


Update NashvilleHousing
SET SoldAsVacant = CASE When SoldAsVacant = 'Y' THEN 'Yes'
	   When SoldAsVacant = 'N' THEN 'No'
	   ELSE SoldAsVacant
	   END

	   -----------------------------------------------------------------------------------------------------------------------------------------------------------

-- Remove Duplicates

	WITH RowNumCTE AS (
select *, 
ROW_NUMBER() OVER (
	PARTITION BY ParcelID,
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) row_num
from PorfolioProject.dbo.NashvilleHousing
--order by ParcelID
)
Select *
From RowNumCTE
Where row_num > 1
Order by PropertyAddress



-- Delete Unused Columns

Select *
from PorfolioProject.dbo.NashvilleHousing


ALTER TABLE PorfolioProject.dbo.NashvilleHousing
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress, SaleDate
