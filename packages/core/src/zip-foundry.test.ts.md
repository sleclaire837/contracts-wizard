# Snapshot report for `src/zip-foundry.test.ts`

The actual snapshot is saved in `zip-foundry.test.ts.snap`.

Generated by [AVA](https://avajs.dev).

## erc20 full

> Snapshot 1

    [
      `#!/usr/bin/env bash␊
      ␊
      # Check if git is installed␊
      if ! which git &> /dev/null␊
      then␊
        echo "git command not found. Install git and try again."␊
        exit 1␊
      fi␊
      ␊
      # Check if Foundry is installed␊
      if ! which forge &> /dev/null␊
      then␊
        echo "forge command not found. Install Foundry and try again. See https://book.getfoundry.sh/getting-started/installation"␊
        exit 1␊
      fi␊
      ␊
      # Setup Foundry project␊
      if ! [ -f "foundry.toml" ]␊
      then␊
        echo "Initializing Foundry project..."␊
      ␊
        # Initialize sample Foundry project␊
        forge init --force --no-commit --quiet␊
      ␊
        # Install OpenZeppelin Contracts␊
        forge install OpenZeppelin/openzeppelin-contracts@vX.Y.Z --no-commit --quiet␊
      ␊
        # Remove template contracts␊
        rm src/Counter.sol␊
        rm script/Counter.s.sol␊
        rm test/Counter.t.sol␊
      ␊
        # Add remappings␊
        if [ -f "remappings.txt" ]␊
        then␊
          echo "" >> remappings.txt␊
        fi␊
        echo "@openzeppelin/contracts/=lib/openzeppelin-contracts/contracts/" >> remappings.txt␊
      ␊
        # Perform initial git commit␊
        git add .␊
        git commit -m "openzeppelin: add wizard output" --quiet␊
      ␊
        echo "Done."␊
      else␊
        echo "Foundry project already initialized."␊
      fi␊
      `,
      `# Sample Foundry Project␊
      ␊
      This project demonstrates a basic Foundry use case. It comes with a contract generated by [OpenZeppelin Wizard](https://wizard.openzeppelin.com/), a test for that contract, and a script that deploys that contract.␊
      ␊
      ## Installing Foundry␊
      ␊
      See [Foundry installation guide](https://book.getfoundry.sh/getting-started/installation).␊
      ␊
      ## Initializing the project␊
      ␊
      \`\`\`␊
      bash setup.sh␊
      \`\`\`␊
      ␊
      ## Testing the contract␊
      ␊
      \`\`\`␊
      forge test␊
      \`\`\`␊
      ␊
      ## Deploying the contract␊
      ␊
      You can simulate a deployment by running the script:␊
      ␊
      \`\`\`␊
      forge script script/MyToken.s.sol␊
      \`\`\`␊
      ␊
      See [Solidity scripting guide](https://book.getfoundry.sh/tutorials/solidity-scripting) for more information.␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Script.sol";␊
      import "../src/MyToken.sol";␊
      ␊
      contract MyTokenScript is Script {␊
        function setUp() public {}␊
      ␊
        function run() public {␊
          // TODO: Set addresses for the contract arguments below, then uncomment the following lines␊
          // vm.startBroadcast();␊
          // MyToken instance = new MyToken(defaultAdmin, pauser, minter);␊
          // console.log("Contract deployed to %s", address(instance));␊
          // vm.stopBroadcast();␊
        }␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      // Compatible with OpenZeppelin Contracts ^5.0.0␊
      pragma solidity ^0.8.20;␊
      ␊
      import "@openzeppelin/contracts/token/ERC20/ERC20.sol";␊
      import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";␊
      import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Pausable.sol";␊
      import "@openzeppelin/contracts/access/AccessControl.sol";␊
      import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Permit.sol";␊
      import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Votes.sol";␊
      import "@openzeppelin/contracts/token/ERC20/extensions/ERC20FlashMint.sol";␊
      ␊
      contract MyToken is ERC20, ERC20Burnable, ERC20Pausable, AccessControl, ERC20Permit, ERC20Votes, ERC20FlashMint {␊
          bytes32 public constant PAUSER_ROLE = keccak256("PAUSER_ROLE");␊
          bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");␊
      ␊
          constructor(address defaultAdmin, address pauser, address minter)␊
              ERC20("My Token", "MTK")␊
              ERC20Permit("My Token")␊
          {␊
              _grantRole(DEFAULT_ADMIN_ROLE, defaultAdmin);␊
              _grantRole(PAUSER_ROLE, pauser);␊
              _mint(msg.sender, 2000 * 10 ** decimals());␊
              _grantRole(MINTER_ROLE, minter);␊
          }␊
      ␊
          function pause() public onlyRole(PAUSER_ROLE) {␊
              _pause();␊
          }␊
      ␊
          function unpause() public onlyRole(PAUSER_ROLE) {␊
              _unpause();␊
          }␊
      ␊
          function mint(address to, uint256 amount) public onlyRole(MINTER_ROLE) {␊
              _mint(to, amount);␊
          }␊
      ␊
          // The following functions are overrides required by Solidity.␊
      ␊
          function _update(address from, address to, uint256 value)␊
              internal␊
              override(ERC20, ERC20Pausable, ERC20Votes)␊
          {␊
              super._update(from, to, value);␊
          }␊
      ␊
          function nonces(address owner)␊
              public␊
              view␊
              override(ERC20Permit, Nonces)␊
              returns (uint256)␊
          {␊
              return super.nonces(owner);␊
          }␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Test.sol";␊
      import "../src/MyToken.sol";␊
      ␊
      contract MyTokenTest is Test {␊
        MyToken public instance;␊
      ␊
        function setUp() public {␊
          address defaultAdmin = vm.addr(1);␊
          address pauser = vm.addr(2);␊
          address minter = vm.addr(3);␊
          instance = new MyToken(defaultAdmin, pauser, minter);␊
        }␊
      ␊
        function testName() public {␊
          assertEq(instance.name(), "My Token");␊
        }␊
      }␊
      `,
    ]

## erc721 basic

> Snapshot 1

    [
      `#!/usr/bin/env bash␊
      ␊
      # Check if git is installed␊
      if ! which git &> /dev/null␊
      then␊
        echo "git command not found. Install git and try again."␊
        exit 1␊
      fi␊
      ␊
      # Check if Foundry is installed␊
      if ! which forge &> /dev/null␊
      then␊
        echo "forge command not found. Install Foundry and try again. See https://book.getfoundry.sh/getting-started/installation"␊
        exit 1␊
      fi␊
      ␊
      # Setup Foundry project␊
      if ! [ -f "foundry.toml" ]␊
      then␊
        echo "Initializing Foundry project..."␊
      ␊
        # Initialize sample Foundry project␊
        forge init --force --no-commit --quiet␊
      ␊
        # Install OpenZeppelin Contracts␊
        forge install OpenZeppelin/openzeppelin-contracts@vX.Y.Z --no-commit --quiet␊
      ␊
        # Remove template contracts␊
        rm src/Counter.sol␊
        rm script/Counter.s.sol␊
        rm test/Counter.t.sol␊
      ␊
        # Add remappings␊
        if [ -f "remappings.txt" ]␊
        then␊
          echo "" >> remappings.txt␊
        fi␊
        echo "@openzeppelin/contracts/=lib/openzeppelin-contracts/contracts/" >> remappings.txt␊
      ␊
        # Perform initial git commit␊
        git add .␊
        git commit -m "openzeppelin: add wizard output" --quiet␊
      ␊
        echo "Done."␊
      else␊
        echo "Foundry project already initialized."␊
      fi␊
      `,
      `# Sample Foundry Project␊
      ␊
      This project demonstrates a basic Foundry use case. It comes with a contract generated by [OpenZeppelin Wizard](https://wizard.openzeppelin.com/), a test for that contract, and a script that deploys that contract.␊
      ␊
      ## Installing Foundry␊
      ␊
      See [Foundry installation guide](https://book.getfoundry.sh/getting-started/installation).␊
      ␊
      ## Initializing the project␊
      ␊
      \`\`\`␊
      bash setup.sh␊
      \`\`\`␊
      ␊
      ## Testing the contract␊
      ␊
      \`\`\`␊
      forge test␊
      \`\`\`␊
      ␊
      ## Deploying the contract␊
      ␊
      You can simulate a deployment by running the script:␊
      ␊
      \`\`\`␊
      forge script script/MyToken.s.sol␊
      \`\`\`␊
      ␊
      See [Solidity scripting guide](https://book.getfoundry.sh/tutorials/solidity-scripting) for more information.␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Script.sol";␊
      import "../src/MyToken.sol";␊
      ␊
      contract MyTokenScript is Script {␊
        function setUp() public {}␊
      ␊
        function run() public {␊
          vm.startBroadcast();␊
          MyToken instance = new MyToken();␊
          console.log("Contract deployed to %s", address(instance));␊
          vm.stopBroadcast();␊
        }␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      // Compatible with OpenZeppelin Contracts ^5.0.0␊
      pragma solidity ^0.8.20;␊
      ␊
      import "@openzeppelin/contracts/token/ERC721/ERC721.sol";␊
      ␊
      contract MyToken is ERC721 {␊
          constructor() ERC721("My Token", "MTK") {}␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Test.sol";␊
      import "../src/MyToken.sol";␊
      ␊
      contract MyTokenTest is Test {␊
        MyToken public instance;␊
      ␊
        function setUp() public {␊
          instance = new MyToken();␊
        }␊
      ␊
        function testName() public {␊
          assertEq(instance.name(), "My Token");␊
        }␊
      }␊
      `,
    ]

## erc1155 basic

> Snapshot 1

    [
      `#!/usr/bin/env bash␊
      ␊
      # Check if git is installed␊
      if ! which git &> /dev/null␊
      then␊
        echo "git command not found. Install git and try again."␊
        exit 1␊
      fi␊
      ␊
      # Check if Foundry is installed␊
      if ! which forge &> /dev/null␊
      then␊
        echo "forge command not found. Install Foundry and try again. See https://book.getfoundry.sh/getting-started/installation"␊
        exit 1␊
      fi␊
      ␊
      # Setup Foundry project␊
      if ! [ -f "foundry.toml" ]␊
      then␊
        echo "Initializing Foundry project..."␊
      ␊
        # Initialize sample Foundry project␊
        forge init --force --no-commit --quiet␊
      ␊
        # Install OpenZeppelin Contracts␊
        forge install OpenZeppelin/openzeppelin-contracts@vX.Y.Z --no-commit --quiet␊
      ␊
        # Remove template contracts␊
        rm src/Counter.sol␊
        rm script/Counter.s.sol␊
        rm test/Counter.t.sol␊
      ␊
        # Add remappings␊
        if [ -f "remappings.txt" ]␊
        then␊
          echo "" >> remappings.txt␊
        fi␊
        echo "@openzeppelin/contracts/=lib/openzeppelin-contracts/contracts/" >> remappings.txt␊
      ␊
        # Perform initial git commit␊
        git add .␊
        git commit -m "openzeppelin: add wizard output" --quiet␊
      ␊
        echo "Done."␊
      else␊
        echo "Foundry project already initialized."␊
      fi␊
      `,
      `# Sample Foundry Project␊
      ␊
      This project demonstrates a basic Foundry use case. It comes with a contract generated by [OpenZeppelin Wizard](https://wizard.openzeppelin.com/), a test for that contract, and a script that deploys that contract.␊
      ␊
      ## Installing Foundry␊
      ␊
      See [Foundry installation guide](https://book.getfoundry.sh/getting-started/installation).␊
      ␊
      ## Initializing the project␊
      ␊
      \`\`\`␊
      bash setup.sh␊
      \`\`\`␊
      ␊
      ## Testing the contract␊
      ␊
      \`\`\`␊
      forge test␊
      \`\`\`␊
      ␊
      ## Deploying the contract␊
      ␊
      You can simulate a deployment by running the script:␊
      ␊
      \`\`\`␊
      forge script script/MyToken.s.sol␊
      \`\`\`␊
      ␊
      See [Solidity scripting guide](https://book.getfoundry.sh/tutorials/solidity-scripting) for more information.␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Script.sol";␊
      import "../src/MyToken.sol";␊
      ␊
      contract MyTokenScript is Script {␊
        function setUp() public {}␊
      ␊
        function run() public {␊
          // TODO: Set addresses for the contract arguments below, then uncomment the following lines␊
          // vm.startBroadcast();␊
          // MyToken instance = new MyToken(initialOwner);␊
          // console.log("Contract deployed to %s", address(instance));␊
          // vm.stopBroadcast();␊
        }␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      // Compatible with OpenZeppelin Contracts ^5.0.0␊
      pragma solidity ^0.8.20;␊
      ␊
      import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";␊
      import "@openzeppelin/contracts/access/Ownable.sol";␊
      ␊
      contract MyToken is ERC1155, Ownable {␊
          constructor(address initialOwner)␊
              ERC1155("https://myuri/{id}")␊
              Ownable(initialOwner)␊
          {}␊
      ␊
          function setURI(string memory newuri) public onlyOwner {␊
              _setURI(newuri);␊
          }␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Test.sol";␊
      import "../src/MyToken.sol";␊
      ␊
      contract MyTokenTest is Test {␊
        MyToken public instance;␊
      ␊
        function setUp() public {␊
          address initialOwner = vm.addr(1);␊
          instance = new MyToken(initialOwner);␊
        }␊
      ␊
        function testUri() public {␊
          assertEq(instance.uri(0), "https://myuri/{id}");␊
        }␊
      }␊
      `,
    ]

## custom basic

> Snapshot 1

    [
      `#!/usr/bin/env bash␊
      ␊
      # Check if git is installed␊
      if ! which git &> /dev/null␊
      then␊
        echo "git command not found. Install git and try again."␊
        exit 1␊
      fi␊
      ␊
      # Check if Foundry is installed␊
      if ! which forge &> /dev/null␊
      then␊
        echo "forge command not found. Install Foundry and try again. See https://book.getfoundry.sh/getting-started/installation"␊
        exit 1␊
      fi␊
      ␊
      # Setup Foundry project␊
      if ! [ -f "foundry.toml" ]␊
      then␊
        echo "Initializing Foundry project..."␊
      ␊
        # Initialize sample Foundry project␊
        forge init --force --no-commit --quiet␊
      ␊
        # Install OpenZeppelin Contracts␊
        forge install OpenZeppelin/openzeppelin-contracts@vX.Y.Z --no-commit --quiet␊
      ␊
        # Remove template contracts␊
        rm src/Counter.sol␊
        rm script/Counter.s.sol␊
        rm test/Counter.t.sol␊
      ␊
        # Add remappings␊
        if [ -f "remappings.txt" ]␊
        then␊
          echo "" >> remappings.txt␊
        fi␊
        echo "@openzeppelin/contracts/=lib/openzeppelin-contracts/contracts/" >> remappings.txt␊
      ␊
        # Perform initial git commit␊
        git add .␊
        git commit -m "openzeppelin: add wizard output" --quiet␊
      ␊
        echo "Done."␊
      else␊
        echo "Foundry project already initialized."␊
      fi␊
      `,
      `# Sample Foundry Project␊
      ␊
      This project demonstrates a basic Foundry use case. It comes with a contract generated by [OpenZeppelin Wizard](https://wizard.openzeppelin.com/), a test for that contract, and a script that deploys that contract.␊
      ␊
      ## Installing Foundry␊
      ␊
      See [Foundry installation guide](https://book.getfoundry.sh/getting-started/installation).␊
      ␊
      ## Initializing the project␊
      ␊
      \`\`\`␊
      bash setup.sh␊
      \`\`\`␊
      ␊
      ## Testing the contract␊
      ␊
      \`\`\`␊
      forge test␊
      \`\`\`␊
      ␊
      ## Deploying the contract␊
      ␊
      You can simulate a deployment by running the script:␊
      ␊
      \`\`\`␊
      forge script script/MyContract.s.sol␊
      \`\`\`␊
      ␊
      See [Solidity scripting guide](https://book.getfoundry.sh/tutorials/solidity-scripting) for more information.␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Script.sol";␊
      import "../src/MyContract.sol";␊
      ␊
      contract MyContractScript is Script {␊
        function setUp() public {}␊
      ␊
        function run() public {␊
          vm.startBroadcast();␊
          MyContract instance = new MyContract();␊
          console.log("Contract deployed to %s", address(instance));␊
          vm.stopBroadcast();␊
        }␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      // Compatible with OpenZeppelin Contracts ^5.0.0␊
      pragma solidity ^0.8.20;␊
      ␊
      contract MyContract {␊
      }␊
      `,
      `// SPDX-License-Identifier: MIT␊
      pragma solidity ^0.8.20;␊
      ␊
      import "forge-std/Test.sol";␊
      import "../src/MyContract.sol";␊
      ␊
      contract MyContractTest is Test {␊
        MyContract public instance;␊
      ␊
        function setUp() public {␊
          instance = new MyContract();␊
        }␊
      ␊
        function testSomething() public {␊
          // Add your test here␊
        }␊
      }␊
      `,
    ]
